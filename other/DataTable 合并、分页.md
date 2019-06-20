---
title: DataTable 合并、分页
date: 2017-03-29 10:45:21
tags: CSDN迁移
---
  ```
///两个结构一样的DT合并
DataTable DataTable1 = new DataTable();
DataTable DataTable2 = new DataTable();
DataTable newDataTable = DataTable1.Clone();

object[] obj = new object[newDataTable.Columns.Count];
for (int i = 0; i < DataTable1.Rows.Count; i++)
{
    DataTable1.Rows[i].ItemArray.CopyTo(obj,0);
    newDataTable.Rows.Add(obj);
}

for (int i = 0; i < DataTable2.Rows.Count; i++)
{
    DataTable2.Rows[i].ItemArray.CopyTo(obj,0);
    newDataTable.Rows.Add(obj);
}
```
 
```
//两个结构不同的DT合并
/// <summary>
  /// 将两个列不同的DataTable合并成一个新的DataTable
  /// </summary>
  /// <param name="dt1">表1</param>
  /// <param name="dt2">表2</param>
  /// <param name="DTName">合并后新的表名</param>
  /// <returns></returns>
  private DataTable UniteDataTable( DataTable dt1 ,DataTable dt2 ,string DTName)
  { 
   DataTable dt3 = dt1.Clone();
   for( int i = 0 ;i < dt2.Columns.Count ;i ++ )
   {
    dt3.Columns.Add( dt2.Columns[i].ColumnName ) ;
   }
   object[] obj = new object[dt3.Columns.Count];

   for (int i = 0; i < dt1.Rows.Count; i++)
   {
    dt1.Rows[i].ItemArray.CopyTo(obj,0);
    dt3.Rows.Add(obj);
   }

   if( dt1.Rows.Count >= dt2.Rows.Count )
   {
    for( int i = 0 ;i < dt2.Rows.Count ;i++ )
    {
     for( int j = 0 ;j < dt2.Columns.Count ;j ++ )
     {
      dt3.Rows[i][j+dt1.Columns.Count] = dt2.Rows[i][j].ToString() ;
     }
    }
   }
   else
   {
    DataRow dr3 ;
    for( int i = 0 ;i < dt2.Rows.Count - dt1.Rows.Count ;i ++ )
    {
     dr3 = dt3.NewRow() ;
     dt3.Rows.Add( dr3 ) ;
    }
    for( int i = 0 ;i < dt2.Rows.Count ;i++ )
    {
     for( int j = 0 ;j < dt2.Columns.Count ;j ++ )
     {
      dt3.Rows[i][j+dt1.Columns.Count] = dt2.Rows[i][j].ToString() ;
     }
    }
   }
   dt3.TableName = DTName ; //设置DT的名字
   return dt3 ;
  }
```
 根据两个表的相同字段拼接两个DataTable

 
```
        /// <summary>
        /// 将两个列不同的DataTable通过两个表中对应的关键字值进行拼接
        /// </summary>
        /// <param name="dt1">表1</param>
        /// <param name="dt2">表2</param>
        /// <param name="dt1Name">表1中的关键字</param>
        /// <param name="dt2Name">表2中的关键字</param>
        /// <returns></returns>
        private DataTable UniteDataTable(DataTable dt1, DataTable dt2, string dt1Name, string dt2Name)
        {
            DataTable dt3 = dt1.Copy();
            int index = 0;
            for (int i = 0; i < dt2.Columns.Count; i++)
            {
                index = 0;
                while (dt1.Columns.Contains(dt2.Columns[i].ColumnName+(index==0?"":index.ToString())))
                {
                    index++;
                }
                dt3.Columns.Add(dt2.Columns[i].ColumnName+(index==0?"":index.ToString()));
            }
            object[] obj = new object[dt3.Columns.Count];

            for (int i = 0; i < dt2.Rows.Count; i++)
            {
                DataRow[] dr1 = dt2.Select(dt2Name + "='" + dt3.Rows[i][dt1Name]+"'");
                if (dr1 != null && dr1.Count() > 0)
                {
                    for (int j = 0; j < dr1[0].Table.Columns.Count; j++)
                    {
                        dt3.Rows[i][j + dt1.Columns.Count] = dr1[0][j].ToString();
                    }
                }
            }
            return dt3;
        }
```
 C# DataTable分页处理

 
```
///PageIndex表示第几页，PageSize表示每页的记录数
public DataTable GetPagedTable(DataTable dt, int PageIndex, int PageSize)

        {
            if (PageIndex == 0)
                return dt;//0页代表每页数据，直接返回

            DataTable newdt = dt.Copy();
            newdt.Clear();//copy dt的框架

            int rowbegin = (PageIndex - 1) * PageSize;
            int rowend = PageIndex * PageSize;

            if (rowbegin >= dt.Rows.Count)
                return newdt;//源数据记录数小于等于要显示的记录，直接返回dt

            if (rowend > dt.Rows.Count)
                rowend = dt.Rows.Count;
            for (int i = rowbegin; i <= rowend - 1; i++)
            {
                DataRow newdr = newdt.NewRow();
                DataRow dr = dt.Rows[i];
                foreach (DataColumn column in dt.Columns)
                {
                    newdr[column.ColumnName] = dr[column.ColumnName];
                }
                newdt.Rows.Add(newdr);
            }
            return newdt;
        }
```
   
  