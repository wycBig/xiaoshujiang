``` 
/**
 * 操作Excel工具类
 *
 * @author goodboy
 * @date 2018-12-25 11:09
 */
public class ExcelUtils {

    private static final Logger LOGGER = LoggerFactory.getLogger(ExcelUtils.class);

    /**
     * 读取excel
     *
     * @param filePath     文件全路径
     * @param ignoreRowOne 是否忽略第一行数据
     * @param clazz        模版
     * @return 读取的数据
     */
    public static List<Object> readModel(String filePath, boolean ignoreRowOne, Class clazz) {
        if (StringUtils.isBlank(filePath)) {
            return null;
        }
        String suffix = filePath.substring(filePath.lastIndexOf(".") + 1);
        if (StringUtils.isBlank(suffix)) {
            throw new AppException("文件无后缀，请检查文件路径");
        }
        try (InputStream inputStream = new FileInputStream(new File(filePath))) {
            Sheet sheet;
            if (ignoreRowOne) {
                sheet = new Sheet(1, 1, clazz);
            } else {
                sheet = new Sheet(1, 0, clazz);
            }
            return EasyExcelFactory.read(inputStream, sheet);
        } catch (Exception e) {
            LOGGER.error(ExceptionUtils.getStackTrace(e));
            throw new AppException(e.getMessage());
        }
    }

    /**
     * 读取数据
     *
     * @param filePath 文件路径
     * @param clazz    模板类型
     * @return 读取的数据
     */
    public static List<Object> readModel(String filePath, Class<? extends BaseRowModel> clazz) {
        // 默认忽略表头数据
        return readModel(filePath, true, clazz);
    }

    /**
     * 读取数据
     *
     * @param filePath 文件路径
     * @return 读取的数据
     */
    public static List<Object> readWithOutModel(String filePath) {
        // 默认忽略表头数据
        return readModel(filePath, true, null);
    }

    /**
     * excel导出
     *
     * @param fileName 文件名
     * @param response 响应
     * @param datas    数据
     */
    public static void writeModel(String fileName, HttpServletResponse response, List<?> datas, Map<Integer, Integer> columnWidths) {

        if (datas != null && datas.size() > 0) {
            EasyExcel.write(getOutputStream(fileName, response), datas.get(0).getClass()).sheet("数据导出").doWrite(datas);
//            ExcelWriter writer = EasyExcelFactory.getWriter(getOutputStream(fileName, response));
//            try {
//                Class<? extends BaseRowModel> clz = datas.get(0).getClass();
//                Sheet sheet = new Sheet(1, 0, clz);
//                sheet.setSheetName(fileName);
//                if (columnWidths != null && columnWidths.size() > 0) {
//                    sheet.setColumnWidthMap(columnWidths);
//                }
//                writer.write(datas, sheet);
//            } catch (Exception e) {
//                LOGGER.error(ExceptionUtils.getFullStackTrace(e));
//                throw new AppException(e.getMessage());
//            } finally {
//                writer.finish();
//            }
        }
    }

    /**
     * excel导出
     *
     * @param fileName 文件名
     * @param response 响应
     * @param datas    数据
     */
    public static void writeModel(String fileName, HttpServletResponse response, List<? extends BaseRowModel> datas) {
        writeModel(fileName, response, datas, null);
    }

    /**
     * 导出 Excel ：一个 sheet，带表头
     *
     * @param response  HttpServletResponse
     * @param list      数据 list，每个元素为一个 BaseRowModel
     * @param fileName  导出的文件名
     * @param sheetName 导入文件的 sheet 名
     * @param object    映射实体类，Excel 模型
     */
    public static void writeExcel(HttpServletResponse response, List<? extends BaseRowModel> list,
                                  String fileName, String sheetName, BaseRowModel object) {
        ExcelWriter writer = new ExcelWriter(getOutputStream(fileName, response), ExcelTypeEnum.XLSX);
        Sheet sheet = new Sheet(1, 0, object.getClass());
        sheet.setSheetName(sheetName);
        writer.write(list, sheet);
        writer.finish();
    }

    /**
     * 导出文件时为Writer生成OutputStream
     */
    private static OutputStream getOutputStream(String fileName, HttpServletResponse response) {
        //创建本地文件
        String filePath = fileName + ".xlsx";
        try {
            fileName = new String(filePath.getBytes(), StandardCharsets.ISO_8859_1);
            response.addHeader("Content-Disposition", "attachment;filename=" + fileName);
            response.setContentType("application/octet-stream;charset=UTF-8");
            return response.getOutputStream();
        } catch (IOException e) {
            throw new AppException("创建文件失败！");
        }
    }

    /**
     * excel导出
     *
     * @param fileName 文件名
     * @param response 响应
     * @param datas    数据
     */
    public static void writeModel(String fileName, TableStyle tableStyle, HttpServletResponse response, List<? extends BaseRowModel> datas, Map<Integer, Integer> columnWidths) {
        if (datas != null && datas.size() > 0) {
            ExcelWriter writer = EasyExcelFactory.getWriter(getOutputStream(fileName, response));
            try {
                Class<? extends BaseRowModel> clz = datas.get(0).getClass();
                Sheet sheet = new Sheet(1, 0, clz);
                sheet.setSheetName(fileName);
                if (columnWidths != null && columnWidths.size() > 0) {
                    sheet.setColumnWidthMap(columnWidths);
                }
                if (tableStyle != null) {
                    sheet.setTableStyle(tableStyle);
                }
                writer.write(datas, sheet);
            } catch (Exception e) {
                LOGGER.error(ExceptionUtils.getStackTrace(e));
                throw new AppException(e.getMessage());
            } finally {
                writer.finish();
            }
        }
    }
}
```