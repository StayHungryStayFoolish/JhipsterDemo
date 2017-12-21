# MapStruct 结合 JHipster

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

1a:1b **Entity、DTO、Mapper 都有 1a (从表) 来维护关系**

    JDL

```
    // 从表 { 主表 } to 主表
   relationship OneToOne {
   	Department{location} to Location
   } 
```

    Entity 从表 维护关系
    
```java
    /*********** 从表 Department   *************/
    @Entity
    @Table(name = "department")
    public class Department implements Serializable {
        
        // DTO id 作为 域 出现
        @OneToOne
        @JoinColumn(unique = true)
        private Location location;
        
        /**
         * 1:N
         * (1端)Department  (N 端)Employee
         * 1端来维护关系，在 DTO 中，Employee 不在出现，
         * 会在 N 端的 Employee 出现一个域 departmentId。 ---------------
        */
        @ApiModelProperty(value = "A relationship")
        @OneToMany(mappedBy = "department")
        @JsonIgnore
        private Set<Employee> employees = new HashSet<>();
    }
    
    /*********** 主表 Location   *************/
    @Entity
    @Table(name = "location")
    public class Location implements Serializable {
    
        // 并不维护任何从表信息
    }
```    

    DTO 从表 维护关系

```java
    /*********** 从表 DepartmentDTO   *************/
    public class DepartmentDTO implements Serializable {
            
        // 主表 Entity ID
        private Long locationId;
        
        // 多端的 Employee 不会在 DTO 中维护，
        // 在 EmployeeDto 使用 1端的 id 作为域维护
    }
    
    /*********** 主表 LocationDTO   *************/
    public class LocationDTO implements Serializable {
        
        // 并不维护任何从表信息
    }
```    

    Mapper 
    1:1 从表Mapper 使用 source 来维护映射关系
    1:N 1 端和 N 端都需要 source 来维护映射关系

```java
    /*********** 从表 DepartmentMapper   *************/
    
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
    
    /*********** 主表 LocationMapper   *************/
    @Mapper(componentModel = "spring", uses = {CountryMapper.class})
    public interface LocationMapper extends EntityMapper<LocationDTO, Location> {
        
        // 并不维护从表任何信息，注释是维护的别的关系
    }
```        

1:N  

- Entity 由 `1端` 维护关系，不同别的是多端会有一个域的存在

- DTO 1端不维护关系，N 端会出现 1端的 id 域

- Mapper 由 `1端` 维护关系 ,uses 既是维护关系的，具体的 source/target 根据转换类型结构确定 

    JDL  为了示范两种关系，分别起名 A / B
```
    relationship OneToMany {
    
        /**
         * 关系 A
        */
    	Employee{job} to Job,
    	/**
         * 关系 B
        */
    	Department{employee} to Employee
    
    }
```

    Entity 
    
```java
    /*********** A 关系 1 端 Employee   *************/
    
    @Entity
    @Table(name = "employee")
    public class Employee implements Serializable {
         
         @OneToMany(mappedBy = "employee")
         @JsonIgnore
         private Set<Job> jobs = new HashSet<>();
         
         // 关系 B 的 N 端
         @ManyToOne
         private Department department;
    }
    
    /*********** A 关系 N 端 Job   *************/
    @Entity
    @Table(name = "job")
    public class Job implements Serializable {
        
         @ManyToOne
         private Employee employee;
    }
    
    
    /*********** B 关系 1 端 Department   *************/
    @Entity
    @Table(name = "department")
    public class Department implements Serializable {
        
        @ApiModelProperty(value = "A relationship") // JDL 注释产生
        @OneToMany(mappedBy = "department")
        @JsonIgnore
        private Set<Employee> employees = new HashSet<>();
    }
    
    /*********** B 关系 N 端 Employee   *************/
    @Entity
    @Table(name = "employee")
    public class Employee implements Serializable {
        
        // 关系 B 的 N 端
        @ManyToOne
        private Department department;
    }
```    

    DTO 1端 不维护， N 端出现 (1端)id域 
    
```java
    /*********** A 关系 1 端 EmployeeDTO   *************/
    
    public class EmployeeDTO implements Serializable {
        
        // 1 端不做任何维护
    }
    
    /*********** A 关系 N 端 JobDTO   *************/
    public class JobDTO implements Serializable {
            
        // N 端出现 (1端)id域 
        private Long employeeId;
    }
    
    /*********** B 关系 1 端 DepartmentDTO   *************/
    public class DepartmentDTO implements Serializable {
        
        // 1 端不做任何维护
    }
    
    public class EmployeeDTO implements Serializable {
            
        // N 端出现 (1端)id域 
        private Long departmentId;
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

    Entity 
    
```java
    /*********** N1 Job   *************/
    @Entity
    @Table(name = "job")
    public class Job implements Serializable {
            
            // (N1) Job 来定义关联关系
            // 由于没有 mappedBy 属性，生成了一个中间的关系表  job_task 数据库关系中间表
            @ManyToMany
            @JoinTable(name = "job_task",
                       joinColumns = @JoinColumn(name="jobs_id", referencedColumnName="id"),
                       inverseJoinColumns = @JoinColumn(name="tasks_id", referencedColumnName="id"))
            private Set<Task> tasks = new HashSet<>();

    }
    
    /*********** N2 Task   *************/
    @Entity
    @Table(name = "task")
    public class Task implements Serializable {
    
        // mappedBy 防止 Task 向 Job 产生中间关系表
        @ManyToMany(mappedBy = "tasks")
        @JsonIgnore
        private Set<Job> jobs = new HashSet<>();
    }
```    

    DTO (N1) Job 维护关系 
    
```java
    public class JobDTO implements Serializable {
        // （N2）TaskDTO 作为域 
        private Set<TaskDTO> tasks = new HashSet<>();
    }
    
    public class TaskDTO implements Serializable {
    
        // 并没有 JobDTO 作为任何 域
    }
```    


## Entity  Class 结构
	
	@ApiModel(description = "JHipster jh 文件生成是附带，表示描述类信息")
	@Entity
	@Table(name="数据库表")
	public class EntityName implements Serializable{
		
		// @Size(min = 10,max = 20) 验证对象是否再给定范围
		// @Null 反正是否为空
		// @NotNull 验证是否为 非空
		// @Pattern(regexp = 表达式) 验证 String 对象 是否符合正则表达规则
		// @Email 验证 String 对象是否符合有效 email 地址规范
		
		
		@Id
		@GeneratedValue(strategy = GenerationType.IDENTITY) // 自增
		private Long id;
		
		@Column(name = "cloumn_name")
		private Type cloumn_name;
		
		// 枚举类型
		@Enumerated(EnumType.STRING)
		private EnumnClass enumn;
		
		/*************  对应关系 ***********************/
		// 1:1 双向关系开启 unique = true
		@OneToOne
	    @JoinColumn(unique = true)
    	private RelateClass instance;
    	
    	// N:1 对应关系，当前类则为 Many	
    	@ManyToOne
	    private OneRelationClass oneClass;
	    
	    // 1:N 对应关系
	    // mapperBy 双向关系中，定义在被拥有方，指向拥有放 N:1 不存在该属性，该属性避免生成中间表
	    // @JsonIgnore 生成 Json 数据时，忽略该属性
	    // @JsonFormat(pattern = “yyyy-MM-dd HH-mm-ss”) 以该模板生成
	    // @Transient 表示该属性并非数据库字段映射。
	    @OneToMany(mappedBy = "employee")
	    @JsonIgnore
    	private Set<Job> jobs = new HashSet<>();
	    
	    /******** N : N 会生成 job_task 关系表，别的不会
	    // N:N 
	    // name = "当前表要连接的表名"
	    // joinColumns 表示可以接受多个 @JoinColumn,用于配置连接表外键信息 name 当前主键，referencedColumnName 外键 ID
	    // inverseJoinColumns 表示可以接受多个 @JoinColumn,用于配置外键对应当前表的主键 name 外键主键，referencedColumnName 当前主键
	    @ManyToMany
	    @JoinTable(name = "job_task",
               joinColumns = @JoinColumn(name="jobs_id", referencedColumnName="id"),
               inverseJoinColumns = @JoinColumn(name="tasks_id", referencedColumnName="id"))
    	private Set<Task> tasks = new HashSet<>();
	    
	    // @ApiModelProperty 对该属性的注解 
	    // value 属性简短性描述
	    // example 属性示例值
	    // required 是否为必须值
        @ApiModelProperty(value = "The firstname attribute.")
	    @Column(name = "first_name")
	    private String firstName;
	    
	    // 拓展 @ApiModel 是对类的描述
	    // value 类的别名，默认为类名
	    // description 类的详细描述
		
		
		/********** 方法 **************/
		getter();
		setter();
		
		@Override
		public boolean equals(Object o){
			比较 id,返回 boolean
		}
		
		@Override
		public int hashCode(){
			返回 id 的 hash
		}
		
		@Override
		public String toString(){
			返回 toString 
		}	
		
	}
	
