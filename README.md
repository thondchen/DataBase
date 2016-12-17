# DataBase

MySQL:
#coalesce
concat(
  '[', coalesce(
    group_concat(concat('[','"',`tblbomdetail`.`oracle_pn`,'"',', ','"',`tblbomdetail`.`sales_partid`,'"',', ','"',`tblbomdetail`.`descr`,'"',']') separator ','),''
  ),']'
) AS `osd`
