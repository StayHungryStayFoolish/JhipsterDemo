# MapStruct

## What`s MapStruct ?

URL: http://mapstruct.org/

MapStruct 是一个代码生成器，简化了 POJO 与 DTO 之间的映射关系。

我们只需要定义一个接口，并注明 `@Mapper()` 然后 `extends` 该接口，即可实现转换。

最低支持 JDK1.6 版本，建议使用最新版本，该插件更新迭代较快，一直在修补各种 Bug，添加功能。

其实现类是在编译时自动生成。文件内找不到，反编译可以查看其源码。

**Maven 依赖**

        <dependency>
            <groupId>org.mapstruct</groupId>
            <artifactId>mapstruct-jdk8</artifactId>
            <version>1.1.0.Final</version>
        </dependency>
        
        <dependency>
            <groupId>org.mapstruct</groupId>
            <artifactId>mapstruct-processor</artifactId>
            <version>1.1.0.Final</version>
            <scope>provided</scope>
        </dependency>
        
## 实现方式

- @Mapper 注解

    1. default 默认情况，不适用任何组件形式，通过 Mappers.getMapper(Class) 方式获取自动生成的实例对象
    
    2. cdi 不太了解，使用 `@Inject` 注入
    
    3. spring 生成的实现类会自动添加 `@Component` ，可以通过 Spring 的 `@Autowired` 注入
    
    4. jsr330 生成实现类自动添加 `@javax.inject.Named` 和 `@Singleton` 注解，通过 `@Inject` 注入 
    
    推荐使用 spring 方式

- 接口示例
    
Mapper 通用接口
    
```java
    public interface EntityMapper <D, E> {
    
        // 以 Entity 结构参考配置 resource 映射关系
        public E toEntity(D dto);
    
        // 以 DTO 结构参考 配置 resource 映射关系
        public D toDto(E entity); 
    
        public List <E> toEntity(List<D> dtoList);
    
        public List <D> toDto(List<E> entityList);
    }
```

1:1 Mapper 接口

    JDL

```
   relationship OneToOne {
   	Department{location} to Location
   } 
```

    Entity 从表 维护关系
    
```java
    @Entity
    @Table(name = "department")
    public class Department implements Serializable {
        
        // DTO id 作为 域 出现
        @OneToOne
        @JoinColumn(unique = true)
        private Location location;
        
        /**
         * A relationship
         * (1端)Department  (N 端)Employee
         * 1端来维护关系，在 DTO 中，Employee 不在出现，
         * 会在 N 端的 Employee 出现一个域 departmentId。 ---------------
        */
        @ApiModelProperty(value = "A relationship")
        @OneToMany(mappedBy = "department")
        @JsonIgnore
        private Set<Employee> employees = new HashSet<>();
    }
```    

    DTO 从表 维护关系

```java
    public class DepartmentDTO implements Serializable {
            
        // 主表 Entity ID
        private Long locationId;
        
        // 多端的 Employee 不会在 DTO 中维护，
        // 在 EmployeeDto 使用 1端的 id 作为域维护
    }
```    

    Mapper 
    1:1 从表Mapper 使用 source 来维护映射关系
    1:N 1 端和 N 端都需要 source 来维护映射关系

```java
    // componentModel = "spring" 采用 spring 实现类方式
    // uses = "LocationMapper.class" 要使用主表关联的 Mapper 内的 Entity、DTO
    @Mapper(componentModel = "spring", uses = {LocationMapper.class})
    public interface DepartmentMapper extends EntityMapper<DepartmentDTO, Department> {
    
        // 1:1 不管在 Entity 还是 DTO 都是由从表来维护关系
        // source = "主表.id" target = "当前从表的域"
        // @Mapping 可以理解为 从 Location 类的 ID 映射到当前类的 locationId 域
        @Mapping(source = "location.id", target = "locationId")
        DepartmentDTO toDto(Department department); 
    
        // (从)Department -> （主）Location 所以要关联
        // source = "Entity 的域，等价于从表 FK"
        // target = "主表对应的 Entity 实例"
        @Mapping(source = "locationId", target = "location")
        // (1端)Department <--> (N端) Employee 
        // 由 1端来维护关系，所以 1端来确定 ignore 
        // Entity 有 Set<Employee> 但是 DTO 不需要，所以 ignore = true
        @Mapping(target = "employees", ignore = true)  
        Department toEntity(DepartmentDTO departmentDTO);
    
        default Department fromId(Long id) {
            if (id == null) {
                return null;
            }
            Department department = new Department();
            department.setId(id);
            return department;
        }
    }
```        

1:N Mapper 接口

    JDL 
```
    relationship OneToMany {
    	Employee{job} to Job,
    	/**
    	* A relationship
    	*/
    	Department{employee} to Employee
    	/**
    	* Another side of the same relationship
    	*/
    	
    }
```
    1 端 Employee Mapper 1 端 Department 见上 1:1 的 Mapper 示例
    
```java
    @Mapper(componentModel = "spring", uses = {DepartmentMapper.class})
    public interface EmployeeMapper extends EntityMapper<EmployeeDTO, Employee> {
        
        // 从Entity 的 Department 映射到 当前 DTO 的 departmentId 域
        @Mapping(source = "department.id", target = "departmentId")
        @Mapping(source = "manager.id", target = "managerId")
        EmployeeDTO toDto(Employee employee); 
    
        // 从 DTO departmentId 映射到 Entity 的 Department 域
        @Mapping(source = "departmentId", target = "department")
        @Mapping(target = "jobs", ignore = true)
        @Mapping(source = "managerId", target = "manager")
        Employee toEntity(EmployeeDTO employeeDTO);
    
        default Employee fromId(Long id) {
            if (id == null) {
                return null;
            }
            Employee employee = new Employee();
            employee.setId(id);
            return employee;
        }
    }

```

N:1 

    JDL 一个经理 管理 N 个员工 
    
```
    relationship ManyToOne {
    	Employee{manager} to Employee
    }
```    

    Entity
    
```java
    @Entity
    @Table(name = "employee")
    public class Employee implements Serializable {
        
        @ManyToOne
        private Employee manager;
        
    }
```    

    DTO
    
```java
    public class EmployeeDTO implements Serializable {
        
        // 1 端作为 id 域
        private Long managerId;
    }
```    

    Mapper 具体看上边 1:N 示例
    
```java
    @Mapper(componentModel = "spring", uses = {DepartmentMapper.class})
    public interface EmployeeMapper extends EntityMapper<EmployeeDTO, Employee> {
           
        @Mapping(source = "managerId", target = "manager")
        Employee toEntity(EmployeeDTO employeeDTO);
    }    
```    

N1:N2

    JDL (N1)Job 来维护关系
    
```
    relationship ManyToMany {
    	Job{task(title)} to Task{job}
    }
```    