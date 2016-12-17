# DataBase

MySQL:
#coalesce
http://stackoverflow.com/questions/15741314/mysql-concat-returns-null-if-any-field-contain-null
http://jackyrong.iteye.com/blog/1547116


ALTER ALGORITHM=UNDEFINED DEFINER=`dev`@`localhost` SQL SECURITY DEFINER VIEW `vw_concatdetail` 
AS 
select `tblbomdetail`.`bom_master_id` AS `bom_master_id1`,
concat(
  '[', 
  coalesce(
    group_concat(
      concat(
        '[',
        '"',coalesce(`tblbomdetail`.`oracle_pn`,''),'"',
        ', ',
        '"',coalesce(`tblbomdetail`.`sales_partid`,''),'"',
        ', ',
        '"',coalesce( `tblbomdetail`.`descr`,''),'"',']'
      ) separator ','
    ),''
  ),']'
) AS `osd`
,group_concat(ifnull(`tblbomdetail`.`oracle_pn`,'') separator '||') 
AS `oracle_pn`,group_concat(ifnull(`tblbomdetail`.`sales_partid`,'') separator '||') 
AS `sales_partid`,group_concat(ifnull(`tblbomdetail`.`descr`,'') separator '||') AS `descr` 
from `tblbomdetail` group by `bom_master_id1`;

