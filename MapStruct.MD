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

```java
    @Mapper(componentModel = "spring", uses = {LocationMapper.class})
    public interface DepartmentMapper extends EntityMapper<DepartmentDTO, Department> {
    
        @Mapping(source = "location.id", target = "locationId")
        DepartmentDTO toDto(Department department); 
    
        @Mapping(source = "locationId", target = "location")
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
