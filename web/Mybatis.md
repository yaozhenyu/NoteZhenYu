# Mybatis

**分页**
```
Page<FilialeDailyDTO> page = new Page<FilialeDailyDTO>(request,response); //new 一个page对象
filialeDailyDTO.setPage(page);//为实体类设置page
page.setList(filialeDailyService.getFilialeDailyList(filialeDailyDTO));//为page设置List
model.addAttribute("page", page);//利用spring的Model对象传递参数到jsp
```
