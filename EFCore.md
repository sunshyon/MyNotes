### **DbFirst**

```shell
Microsoft.EntityFrameworkCore 
Microsoft.EntityFrameworkCore.Design 
Microsoft.EntityFrameworkCore.SqlServer 
Microsoft.EntityFrameworkCore.SqlServer.Design (启动项目)
Microsoft.EntityFrameworkCore.Tools

#生成：
Scaffold-DbContext -Connection "Server=.;Database=dbConnStr;uid=sa;pwd=123" Microsoft.EntityFrameworkCore.SqlServer -OutputDir "Models"

#Migration:
context.Database.EnsureDeleted(); //删除数据库
context.Database.EnsureCreated(); //创建数据库和表
#命令：
add-migration Init -Context xxDbContext –OutputDir Migrations\\xxxx 
update-database -Context xxDbContext
注意：从DbFirst生产而来的，再迁移需要删除第一个迁移文件

```

### **CodeFirst**

```c#
Microsoft.EntityFrameworkCore 
Microsoft.EntityFrameworkCore.SqlServer 
Microsoft.EntityFrameworkCore.Tools 
Microsoft.EntityFrameworkCore.Design (启动项目)

//Dbcontext
public class MyDbContext:DbContext
{
public MyDbContext(DbContextOptions<UserServiceDbContext> options) :base(options)
{}
    public DbSet<User> User { get; set; }
}

//Startup
var conn = Configuration.GetConnectionString("UserServiceDb");
services.AddDbContext<UserServiceDbContext>(opt =>
 {
    opt.UseSqlServer(conn);
  });

//命令：
add-migration Init -Context xxDbContext –OutputDir Migrations\\xxxx 
update-database -Context xxDbContext

```

### **Mysql Dbfirst**

```json
Microsoft.EntityFrameworkCore  3.1.1
MySql.Data.EntityFrameworkCore 注意支持的版本
Microsoft.EntityFrameworkCore.Design (启动项目)  3.1.1
"ConnectionStrings": {
    "Mysql00": "server=106.14.218.108;database=EFDemo;user=root;pwd=123456;Character Set=utf8;"
  }
注意：存在问题，插入时出现异常，可能是字符集问题
```

### **回退迁移**

```
1、如果迁移未应用到数据库则直接remove-migration  ----删除最新的迁移
2、如果已应用到数据库，先执行update-database "迁移名"，再执行remove-migration
```

