# 导出服务架构分析

ExportDataServiceController export方法用于导出生成文件；

## 第一步

对参数进行校验，不符合条件返回错误信息

## 第二步

通过前台参数构建导出数据（ExportData  -> List<IDataAccessor>/List<ExportTree>）

 通过前台项目的id来获取到对应的excel模板和每个excel需要获取的数据信息，有多少的sheet需要进行填充数据等等

 ExportData主要包括两部分：

 第一部分是：List<IDataAccessor> AccessorList

> AbstractAccessor-》》ExportTree / ExportItem 
>
> > 是一个继承callable的类，进行工作获取远程接口数据 

> ExportItem-》》IDataBinder / ISheetData    
>
> > 导出条目配置对象，一个sheet对应一个Item     

​	Accessor里面存放的有导出树对象（模板名称用于获取模板，导出参数，工件类型等，该往哪个sheet写数据，sheet的数据，获取数据格式化后存入，从哪个接口获取url）是否导出多个excel，和条目配置对象（sheet页的中英文名称，绑定数据的执行器，获取数据的urlkey）将每个树的item都放到其中，启动线程的时候就是获取这个列表来决定开启的线程

​	第二部分是：List<ExportTree> exportTreeList

> ExportTree ->> List**<**ExportCategory> / ExcelTemplate 
>
> > 一个excel对应一个导出tree                  



>  ExportCategory ->> List**<**ExportItem**>**        
>
> > 一组sheet页对应一个category           

> ExportItem ->> IDataBinder/ISheetData           
>
> > 一个sheet对应一个Item，里面有绑定数据的binder和通过accessor获取到的IsheetData 

​	每一个导出的excel都有对应的一个导出树对象（模板名称用于获取模板，导出参数，工件类型等）是否导出多个excel

## 第三步

构件好导出树之后就可以根据导出树的数据调用多线程任务去启动获取数据的服务

 初始化好线程池：50个线程数，LinkedBlockingQueue阻塞队列

 executorPool.submit(accessor)提交每个accessor

 IDataAccessor继承Callable，可被调用获取数据，基本上每个sheet页都需要一个Accessor去对应的模块获取数据。当然也封装了一些通用的模板去获取数据，例如有些sheet页面只有一个表格，那么这种sheet也就可以封装一个统一的accessor去获取表格数据，表格的样式都统一，动态根据配置获取起始行列来生成sheet页。

 定义抽象类AbstractAccessor实现IDataAccessor公共方法实现在抽象类，项目中所有的Accessor都继承抽象类，实现特有的从不同的url获取不同格式的数据功能，这就是使用了java设计模式中的模板模式

 accessor的call方法获取到的的是一个ISHeetData接口，IsheetData有很多实现类（里面的信息包括起始行，起始列，sheet表的样式信息，表头信息表格标题等等，还有一些仅仅是一个起始行起始列和一个Workbook的excel文件，存放图片列表的sheet）是每个sheet页的数据ISheetData放到Accessor

## 第四步

获取完数据所有的数据我们的导出树就已经完整了，里面已经有了数据

 遍历里面的ExportItem把相关的数据样式还有位置信息等等都拿出来，插入到我们的模板中

 AbstractBinder用于插数据到excel中，bind（）方法传入的参数是ISheetData。单个表格插入，图片列表插入，自定义数据插入的数据绑定器。

 绑定完数据之后就把生成好的LLD文档和ZTP文档放到服务器的临时目录下，等待用户调用下载

 使用aspose.cells操作

