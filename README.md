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


#vw_mgr_apvdoc
CREATE ALGORITHM=UNDEFINED DEFINER=`dev`@`localhost` SQL SECURITY DEFINER VIEW `vw_mgr_apvdoc` AS 
select `b`.`bom_master_id` AS `bom_master_id`,
`b`.`req_date` AS `req_date`,
`b`.`bom_no` AS `bom_no`,
`b`.`version` AS `version`,
`b`.`stage` AS `stage`,
`b`.`status` AS `status`,
`b`.`requester` AS `requester`,
`b`.`class_id` AS `class_id`,
`b`.`pm_name` AS `pm_name`,
`b`.`req_type` AS `req_type`,
`b`.`eco` AS `eco`,
`b`.`reason` AS `reason`,
`b`.`prod_family` AS `prod_family`,
`b`.`approve_date` AS `approve_date`,
`b`.`reject_date` AS `reject_date`,
`b`.`finish_date` AS `finish_date`,
`b`.`cancel_date` AS `cancel_date`,
`b`.`cancel_reason` AS `cancel_reason`,
`b`.`canceller` AS `canceller`,
`b`.`remark` AS `remark`,
`b`.`change_notice` AS `change_notice`,
`b`.`filename` AS `attachfile`,
`b`.`activeflag` AS `activeflag`,
`b`.`createduser` AS `createduser`,
`b`.`createdtime` AS `createdtime`,
`b`.`revisedby` AS `revisedby`,
`b`.`revisedtime` AS `revisedtime`,
`b`.`osd` AS `osd`,`b`.`oracle_pn` AS `oracle_pn`,
`b`.`sales_partid` AS `sales_partid`,
`b`.`descr` AS `descr`,
`d`.`wf_detail_id` AS `wf_detail_id`,
`d`.`wf_master_id` AS `wf_master_id`,
`d`.`level_sno` AS `level_sno`,
`d`.`approval_emp_id` AS `approval_emp_id`,
`d`.`agent_emp_id` AS `agent_emp_id`,
`d`.`approval_status` AS `approval_status`,
`d`.`comment` AS `comment`,
`d`.`authorize_emp_id` AS `authorize_emp_id`,
`d`.`emailagent_emp_id` AS `emailagent_emp_id`,
`d`.`emailagent_emp_id_bk` AS `emailagent_emp_id_bk`,
`d`.`level_final_flag` AS `level_final_flag`,
`d`.`level_current_flag` AS `level_current_flag`,
`d`.`create_date` AS `create_date`,
`d`.`open_authorize_date` AS `open_authorize_date`,
`d`.`authorize_date` AS `authorize_date`,
`d`.`filename` AS `filename`,
concat_ws(',',`d`.`approval_emp_id`,`d`.`agent_emp_id`,`d`.`emailagent_emp_id`,`d`.`emailagent_emp_id_bk`) AS `blendapprovar` 
from (`vw_bomformdetail` `b` join `com_wf_detail` `d`) where ((`b`.`wf_master_id` = `d`.`wf_master_id`) and (`b`.`status` in ('initiated','approved')) and (`d`.`approval_status` = 'initiated') and (`b`.`activeflag` = 'y'));

