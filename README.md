### 系统介绍

基于SpringBoot和Vue实现的作业管理系统采用前后端分离的架构方式，系统设计了三种角色，分别是学生、教师、管理员，每种角色拥有不同的菜单权限，学生可以在系统中进行注册、登录、个人信息、首页、课程选课、我的课程、我的作业、文章专栏、通知公告等功能模块的操作，教师可以在系统中进行注册、登录、个人信息、首页、教学管理、作业管理等功能模块的操作，管理员可以在系统中进行注册、登录、个人信息、首页、管理员管理、教师管理、学生管理、教学管理、作业管理、通知公告、文章管理、评论管理、轮播图管理等功能模块的操作。

### 技术选型

开发工具：idea2020.3+webstorm2020.3

运行环境：jdk1.8+maven3.6.3+mysql8.0+nodejs18.16.0

服务端技术：springboot+mybatis-plus+fastjson+jwt

前端技术：html+css+vue+axios+element-ui+echarts

### 成果展示

系统登录
<img width="1873" height="1018" alt="用户登录" src="https://github.com/user-attachments/assets/d458e1c6-2769-48fd-9dc7-9ce0a19a751d" />

学生->首页
<img width="1894" height="1024" alt="学生-首页" src="https://github.com/user-attachments/assets/98b1460f-504c-4951-95e9-1a04645ebe75" />

学生->课程选课
<img width="1891" height="1019" alt="学生-课程选课" src="https://github.com/user-attachments/assets/378b5306-c6a0-451c-9901-9af13bdbdbae" />

学生->我的课程
<img width="1899" height="1019" alt="学生-我的课程" src="https://github.com/user-attachments/assets/583d0f93-949c-4825-9b6e-e0285669e1e8" />

学生->我的作业 
<img width="1900" height="1055" alt="学生-我的作业" src="https://github.com/user-attachments/assets/8aa5a0a5-1a69-465f-8783-61fc08905822" />
<img width="1893" height="1020" alt="学生-我的作业2" src="https://github.com/user-attachments/assets/57cb963a-a6bc-4a14-8bda-6ae1fee293f7" />

学生->文章专栏 
<img width="1895" height="1023" alt="学生-文章专栏" src="https://github.com/user-attachments/assets/73201419-b242-4a10-bb06-e84126de3753" />
<img width="1898" height="1024" alt="学生-文章专栏2" src="https://github.com/user-attachments/assets/e966b4f2-466f-4940-b546-acfd63735a71" />

学生->通知公告
<img width="1895" height="1018" alt="学生-通知公告" src="https://github.com/user-attachments/assets/b1e9da52-d883-48e1-a6cc-9ed8a0a66950" />

教师->教学管理
<img width="1896" height="1049" alt="教师-教学管理" src="https://github.com/user-attachments/assets/580870a4-b60d-452b-8bbd-d08253101e39" />

教师->作业管理->作业发布
<img width="1900" height="1049" alt="教师-作业管理-作业发布" src="https://github.com/user-attachments/assets/2bc19e60-a7aa-44a8-b824-2e7a4c473539" />

教师->作业管理->作业批改
<img width="1901" height="1040" alt="教师-作业管理-作业批改" src="https://github.com/user-attachments/assets/5421e40f-c361-4cfd-81e4-630a35be10a3" />

管理员->首页
<img width="1895" height="1029" alt="管理员-首页" src="https://github.com/user-attachments/assets/79dcdef5-411c-4de7-a481-7dcdebca6766" />

管理员->教师管理
<img width="1894" height="1023" alt="管理员-教师管理" src="https://github.com/user-attachments/assets/f45e7e46-3fb2-412a-bf1f-a9deb4c134ca" />

管理员->学生管理
<img width="1895" height="1024" alt="管理员-学生管理" src="https://github.com/user-attachments/assets/8f1c8af0-feba-4f6a-a44b-3f7f99764350" />

管理员->教学管理
<img width="1891" height="1020" alt="管理员-教学管理" src="https://github.com/user-attachments/assets/da3effb0-d645-4dbb-b80d-99afd4e4cae4" />

管理员->通知公告
<img width="1894" height="1021" alt="管理员-通知公告" src="https://github.com/user-attachments/assets/0a332469-33bc-4f27-baca-909d40b7348b" />

管理员->文章管理
<img width="1894" height="1021" alt="管理员-文章管理" src="https://github.com/user-attachments/assets/67f2885b-a55a-4f8e-a1eb-6b342f85311f" />

管理员->评论管理
<img width="1894" height="1023" alt="管理员-评论管理" src="https://github.com/user-attachments/assets/65229da3-8c84-4b0a-a2cf-37cd9b297ebe" />

管理员->轮播图管理
<img width="1894" height="1024" alt="管理员-轮播图管理" src="https://github.com/user-attachments/assets/658f5c3e-c7f9-461b-89fd-f85d340862e2" />

### 源码展示

@RestController

@RequestMapping("/course")

public class CourseController {

    @Resource
    private CourseService service;

    /**
     * 分页查询
     */
    @GetMapping("page")
    public ResponseVO<PageVO<Course>> page(@RequestParam Map<String, Object> query, @RequestParam(defaultValue = "1") Integer pageNum, @RequestParam(defaultValue = "10") Integer pageSize) {
        // 教师只能查看自己的课程
        CurrentUserDTO currentUser = CurrentUserThreadLocal.getCurrentUser();
        if ("TEACHER".equals(currentUser.getType())) {
            query.put("teacherId", currentUser.getId());
        }
        PageVO<Course> page = service.page(query, pageNum, pageSize);
        return ResponseVO.ok(page);
    }

    /**
     * 根据id查询
     */
    @GetMapping("selectById/{id}")
    public ResponseVO<Course> selectById(@PathVariable("id") Integer id) {
        Course entity = service.selectById(id);
        return ResponseVO.ok(entity);
    }

    /**
     * 列表
     */
    @GetMapping("list")
    public ResponseVO<List<Course>> list() {
        // 教师只能查看自己的课程
        CurrentUserDTO currentUser = CurrentUserThreadLocal.getCurrentUser();
        if ("TEACHER".equals(currentUser.getType())) {
            return ResponseVO.ok(service.listByTeacherId(currentUser.getId()));
        }
        return ResponseVO.ok(service.list());
    }

    /**
     * 新增
     */
    @PostMapping("add")
    public ResponseVO add(@RequestBody Course entity) {
        service.insert(entity);
        return ResponseVO.ok();
    }

    /**
     * 更新
     */
    @PutMapping("update")
    public ResponseVO update(@RequestBody Course entity) {
        service.updateById(entity);
        return ResponseVO.ok();
    }

    /**
     * 批量删除
     */
    @DeleteMapping("delBatch")
    public ResponseVO delBatch(@RequestBody List<Integer> ids) {
        service.removeByIds(ids);
        return ResponseVO.ok();
    }
}

### 账号地址及其它说明

1、地址说明

登录页：localhost:5173

2、账号说明

学生：user/123456

教师：teacher/123456

管理员：admin/123456

3、目录结构展示

<img width="729" height="176" alt="目录结构" src="https://github.com/user-attachments/assets/b5db018e-2f5f-4a0b-924b-03458587c6d8" />

4、项目结构展示

<img width="1736" height="979" alt="项目结构" src="https://github.com/user-attachments/assets/7171ea2e-4efd-4eb5-9b16-042c805530ca" />

5、运行步骤

1）创建数据库、导入sql脚本

2）修改application.yml中的数据库配置文件，启动服务端

3）在web目录下打开cmd，执行npm install下载依赖

4）下载完毕后启动前端npm run serve，访问端口

### 获取方式(可远程调试)

访问链接(在浏览器中手动输入下图中的地址)：

<img width="1135" height="140" alt="链接" src="https://github.com/user-attachments/assets/1fb70f8b-2a8f-40e6-854d-ffc7248f8897" />

若资源获取失败，可添加happy35596339(vx)或2061772307(qq)进行交流
