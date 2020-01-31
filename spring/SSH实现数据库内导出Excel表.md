# SSH实现数据库内导出Excel表

JSP代码

 

//导出用户到表格

```
functiondoExportExcel(){window.open("${basePath}nsfw/user_exportExcel.action");

}

<input type="button" value="导出" class="s_button" onclick="doExportExcel()"/> 

```

jsp页面要写成form格式

```
<form action="${cxt}/subarea/importExcel" method="post" enctype="multipart/form-data" target="_parent">
	<input type="submit" value="导出" />
</form>
```



Action



```
//导入文件

private File userExcel;//文件"userExcel"

private String userExcelContentType;//文件类型

private String userExcelFileName;// 文件名称

Get Set 方法


//导出用户到表格
	public void exportExcel(){
		try {
			//1 确定需要导出的数据 
			List<User> userList = userService.findObjects();
			if(userList!=null){
				//2 设置响应信息
				HttpServletResponse response = ServletActionContext.getResponse();
					//2.1 设置响应类型
				response.setContentType("application/x-execl");
					//2.2 设置以下载方式打开文件
				response.setHeader("Content-Disposition", "attachment;filename="+new String("用户列表.xls".getBytes(),"ISO-8859-1") );
					//2.3 获得输出流
				OutputStream outputStream = response.getOutputStream();					
				//3 导出
				userService.exportExcel(userList,outputStream);
				if(outputStream != null){
					outputStream.close();
				}				
			}			
		} catch (Exception e) {
			e.printStackTrace();
		}	
	}

```

service
//导出用户列表到表格

```
@Override

public void exportExcel(List<User> userList, OutputStream outputStream) {

ExcelUtil.exportExcel(userList, outputStream);

}



```

ExcelUtil 

public class ExcelUtil {

		/**
		 * 导出用户列表到表格 
		 * @param userList 参数为用户集合列表
		 * @param outputStream 参数为输出流
		 */
		public static void exportExcel(List<User> userList, OutputStream outputStream) {
			try {
				//1  创建工作薄
				HSSFWorkbook workBook = new HSSFWorkbook();
					//1.1  创建合并单元格对象 
				CellRangeAddress cellRangeAddress = new CellRangeAddress(0, 0, 0, 4);				
				//2 创建工作表
				HSSFSheet sheet = workBook.createSheet();
				
					//2.1 加载合并单元格对象
				sheet.addMergedRegion(cellRangeAddress);			
				//3 创建行 
					//3.1头标题行
				HSSFRow rowHead = sheet.createRow(0);
				HSSFCell cellHead = rowHead.createCell(0);		
				cellHead.setCellValue("用户列表");
					//3.1.3将设置好的样式添加到头标题行		
				cellHead.setCellStyle( getStyle(workBook,(short) 16));			
					//3.2 标题行
				HSSFRow rowColumn = sheet.createRow(1);
					//3.2.1 标题数组
				String[] title={"用户名","账号","所属部门","性别","电子邮箱"};
				for(int i=0 ; i< title.length;i++){//遍历数组
					HSSFCell cellColumn = rowColumn.createCell(i);
					//3.2.2 调用方法 设置标题行样式
					cellColumn.setCellStyle(getStyle(workBook,(short) 12));
					//3.2.3 设置标题
					cellColumn.setCellValue(title[i]);
				}			
				//4 操作单元格
				if(userList != null){
					for (int j=0;j<userList.size();j++) {			
						HSSFRow row = sheet.createRow(j+2);
						row.createCell(0).setCellValue(userList.get(j).getName());						
						row.createCell(1).setCellValue(userList.get(j).getAccount());						
						row.createCell(2).setCellValue(userList.get(j).getDept());						
						row.createCell(3).setCellValue(userList.get(j).isGender()?"男":"女");						
						row.createCell(4).setCellValue(userList.get(j).getEmail());						
					}			
				}
				
		        sheet.autoSizeColumn(( short ) 4); // 调整第五列宽度 邮箱列
		        //5、输出
				workBook.write(outputStream);
				workBook.close();
			} catch (Exception e) {
				e.printStackTrace();
			}		
		}
		private static HSSFCellStyle getStyle(HSSFWorkbook workBook,short fontSize){
			HSSFCellStyle style = workBook.createCellStyle();
			//3.1.1设置头标题行居中
			style.setAlignment(HSSFCellStyle.ALIGN_CENTER);
			style.setVerticalAlignment(HSSFCellStyle.ALIGN_CENTER);
				//3.1.2设置头标题行字体加粗居中
			HSSFFont font = workBook.createFont();
			font.setBold(true);
			font.setFontHeightInPoints(fontSize);
			style.setFont(font);
			return style;
		}
		}


————————————————
版权声明：本文为CSDN博主「All-Might」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_37385585/article/details/79661382


