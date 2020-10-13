---
keyword: comparison of built-in functions
---

# Comparison of functions built in MaxCompute, MySQL, and Oracle

This topic compares functions built in MaxCompute, MySQL, and Oracle.

|Function type|MaxCompute|HIVE|MySQL|Oracle|Partition pruning supported in MaxCompute SQL|
|[Date functions](/intl.en-US/Development/SQL/Builtin functions/Date functions.md)|[DATEDIFF](/intl.en-US/Development/SQL/Builtin functions/Date functions.md)|DATEDIFF|DATEDIFF|MONTHS\_BETWEEN|-   Supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[DATE\_ADD](/intl.en-US/Development/SQL/Builtin functions/Date functions.md)|DATE\_ADD|DATE\_ADD|N/A|-   Not supported in MaxCompute mode. We recommend that you use the DATEADD function instead.
-   Supported in Hive-compatible mode |
|[DATEPART](/intl.en-US/Development/SQL/Builtin functions/Date functions.md)|N/A|DATE\_FORMAT|EXTRACT \(datetime\)|-   Supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[DATETRUNC](/intl.en-US/Development/SQL/Builtin functions/Date functions.md)|TRUNC|DATE\_FORMAT|EXTRACT \(datetime\)|-   Supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[FROM\_UNIXTIME](/intl.en-US/Development/SQL/Builtin functions/Date functions.md)|FROM\_UNIXTIME|FROM\_UNIXTIME|N/A|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[GETDATE](/intl.en-US/Development/SQL/Builtin functions/Date functions.md)|CURRENT\_DATE|NOW|CURRENT\_DATE|-   Supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[ISDATE](/intl.en-US/Development/SQL/Builtin functions/Date functions.md)|N/A|STR\_TO\_DATE \(If false is returned, the string cannot be converted to DATE.\)|N/A|-   Supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[LASTDAY](/intl.en-US/Development/SQL/Builtin functions/Date functions.md)|LAST\_DAY|LAST\_DAY|LAST\_DAY|-   Supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[TO\_DATE](/intl.en-US/Development/SQL/Builtin functions/Date functions.md)|TO\_DATE|STR\_TO\_DATE\(\)|DATE|-   Supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[TO\_CHAR](/intl.en-US/Development/SQL/Builtin functions/Date functions.md)|N/A|DATE\_FORMAT|TO\_CHAR \(datetime\)|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[UNIX\_TIMESTAMP](/intl.en-US/Development/SQL/Builtin functions/Date functions.md)|UNIX\_TIMESTAMP|UNIX\_TIMESTAMP|N/A|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[WEEKDAY](/intl.en-US/Development/SQL/Builtin functions/Date functions.md)|N/A|WEEKDAY|N/A|-   Supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[WEEKOFYEAR](/intl.en-US/Development/SQL/Builtin functions/Date functions.md)|WEEKOFYEAR|WEEKOFYEAR|N/A|-   Supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[YEAR](/intl.en-US/Development/SQL/Builtin functions/Date functions.md)|YEAR|YEAR|YEAR|-   Not supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[QUARTER](/intl.en-US/Development/SQL/Builtin functions/Date functions.md)|QUARTER|QUARTER|QUARTER|-   Not supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[MONTH](/intl.en-US/Development/SQL/Builtin functions/Date functions.md)|MONTH|MONTH|MONTH|-   Not supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[DAY](/intl.en-US/Development/SQL/Builtin functions/Date functions.md)|DAY|DAY|DAY|-   Not supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[DAYOFMONTH](/intl.en-US/Development/SQL/Builtin functions/Date functions.md)|DAYOFMONTH|DAYOFMONTH|N/A|-   Not supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[HOUR](/intl.en-US/Development/SQL/Builtin functions/Date functions.md)|HOUR|HOUR|HOUR|-   Not supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[MINUTE](/intl.en-US/Development/SQL/Builtin functions/Date functions.md)|MINUTE|MINUTE|MINUTE|-   Not supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[CURRENT\_TIMESTAMP](/intl.en-US/Development/SQL/Builtin functions/Date functions.md)|CURRENT\_TIMESTAMP|CURRENT\_TIMESTAMP|CURRENT\_TIMESTAMP|-   Not supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[ADD\_MONTHS](/intl.en-US/Development/SQL/Builtin functions/Date functions.md)|ADD\_MONTHS|ADDDATE|ADD\_MONTHS|-   Not supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[LAST\_DAY](/intl.en-US/Development/SQL/Builtin functions/Date functions.md)|LAST\_DAY|LAST\_DAY|N/A|-   Not supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[NEXT\_DAY](/intl.en-US/Development/SQL/Builtin functions/Date functions.md)|NEXT\_DAY|N/A|NEXT\_DAY|-   Not supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[MONTHS\_BETWEEN](/intl.en-US/Development/SQL/Builtin functions/Date functions.md)|MONTHS\_BETWEEN|timestampdiff|MONTHS\_BETWEEN|-   Not supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[Mathematical functions](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.md)|[ABS](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.md)|ABS|ABS|ABS|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[ACOS](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.md)|ACOS|ACOS|ACOS|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[ASIN](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.mdsection_fau_d9e_7p5)|ASIN|ASIN|ASIN|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[ATAN](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.mdsection_odw_jnm_vdb)|ATAN|ATAN|ATAN|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[CEIL](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.md)|CEIL|CEIL|CEIL|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[CONV](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.mdsection_tkx_q4m_vdb)|CONV|CONV|N/A|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[COS](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.md)|COS|COS|COS|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[COSH](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.mdsection_tnp_gpm_vdb)|COSH|N/A|COSH|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[COT](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.md)|COT|COT|COT|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[EXP](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.md)|EXP|EXP|EXP|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[FLOOR](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.md)|FLOOR|FLOOR|FLOOR|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[LN](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.mdsection_pdm_fqm_vdb)|LN|LN|LN|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[LOG](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.md)|LOG|LOG|LOG|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[POW](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.mdsection_gmv_wqm_vdb)|POW|POW|POWER|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[RAND](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.md)|RAND|RAND|N/A|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[ROUND](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.mdsection_ocf_jrm_vdb)|ROUND|ROUND|ROUND|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[SIN](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.md)|SIN|SIN|SIN|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[SINH](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.mdsection_ccf_gym_vdb)|SINH|N/A|SINH|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[SQRT](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.mdsection_nns_lym_vdb)|SQRT|SQRT|SQRT|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[TAN](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.mdsection_ibd_rym_vdb)|TAN|TAN|TAN|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[TANH](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.mdsection_pfh_wym_vdb)|TANH|N/A|TANH|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[TRUNC](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.md)|TRUNC|TRUNCATE|TRUNC|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[LOG2](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.mdsection_dh3_tzm_vdb)|LOG2|LOG2|LOG|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[LOG10](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.md)|LOG10|LOG10|LOG|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[BIN](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.md)|BIN|BIN|BITAND|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[HEX](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.md)|HEX|HEX|RAWTOHEX|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[UNHEX](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.md)|UNHEX|UNHEX|HEXTORAW|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[RADIANS](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.mdsection_dwg_1bn_vdb)|RADIANS|RADIANS|RADIANS|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[DEGREES](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.md)|DEGREES|DEGREES|DEGREES|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[SIGN](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.md)|SIGN|SIGN|SIGN|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[E](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.md)|E|N/A|N/A|-   Supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[PI](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.md)|PI|PI|PI|-   Supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[FACTORIAL](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.md)|FACTORIAL|N/A|N/A|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[CBRT](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.mdsection_frl_lcn_vdb)|CBRT|N/A|N/A|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[SHIFTLEFT](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.mdsection_k4z_pcn_vdb)|SHIFTLEFT|<<|N/A|-   Not supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[SHIFTRIGHT](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.mdsection_iyl_vcn_vdb)|SHIFTRIGHT|\>\>|N/A|-   Not supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[SHIFTRIGHTUNSIGNED](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.md)|SHIFTRIGHTUNSIGNED|\>\>\>|N/A|-   Not supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[Window functions](/intl.en-US/Development/SQL/Builtin functions/Window functions.md)|[DENSE\_RANK](/intl.en-US/Development/SQL/Builtin functions/Window functions.mdsection_mj4_k11_wdb)|DENSE\_RANK|DENSE\_RANK|DENSE\_RANK|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[RANK](/intl.en-US/Development/SQL/Builtin functions/Window functions.mdsection_yvx_jb1_wdb)|RANK|RANK|RANK|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[LAG](/intl.en-US/Development/SQL/Builtin functions/Window functions.mdsection_dbf_xb1_wdb)|LAG|LAG|LAG|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[LEAD](/intl.en-US/Development/SQL/Builtin functions/Window functions.mdsection_s5f_jc1_wdb)|LEAD|LEAD|LEAD|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[PERCENT\_RANK](/intl.en-US/Development/SQL/Builtin functions/Window functions.mdsection_lmk_tc1_wdb)|PERCENT\_RANK|PERCENT\_RANK|PERCENT\_RANK|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[ROW\_NUMBER](/intl.en-US/Development/SQL/Builtin functions/Window functions.mdsection_cm1_cd1_wdb)|ROW\_NUMBER|ROW\_NUMBER|ROW\_NUMBER|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[CLUSTER\_SAMPLE](/intl.en-US/Development/SQL/Builtin functions/Window functions.mdsection_mst_md1_wdb)|N/A|N/A|N/A|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[NTILE](/intl.en-US/Development/SQL/Builtin functions/Window functions.mdsection_gjj_c21_wdb)|NTILE|NTILE|NTILE|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[Aggregate functions](/intl.en-US/Development/SQL/Builtin functions/Aggregate functions.md)|[COUNT](/intl.en-US/Development/SQL/Builtin functions/Aggregate functions.mdsection_x1k_xq1_wdb)|COUNT|COUNT|COUNT|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[AVG](/intl.en-US/Development/SQL/Builtin functions/Aggregate functions.mdsection_oxf_mr1_wdb)|AVG|AVG|AVG|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[MAX](/intl.en-US/Development/SQL/Builtin functions/Aggregate functions.md)|MAX|MAX|MAX|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[MIN](/intl.en-US/Development/SQL/Builtin functions/Aggregate functions.mdsection_mll_yr1_wdb)|MIN|MIN|MIN|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[MEDIAN](/intl.en-US/Development/SQL/Builtin functions/Aggregate functions.mdsection_m5y_cs1_wdb)|N/A|N/A|MEDIAN|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[STDDEV](/intl.en-US/Development/SQL/Builtin functions/Aggregate functions.md)|STDDEV|STDDEV|STDDEV|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[STDDEV\_SAMP](/intl.en-US/Development/SQL/Builtin functions/Aggregate functions.mdsection_sgk_jv1_wdb)|STDDEV\_SAMP|STDDEV\_SAMP|STDDEV\_SAMP|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[SUM](/intl.en-US/Development/SQL/Builtin functions/Aggregate functions.mdsection_okf_4v1_wdb)|SUM|SUM|SUM|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[WM\_CONCAT](/intl.en-US/Development/SQL/Builtin functions/Aggregate functions.md)|N/A|GROUP\_CONCAT|WM\_CONCAT|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[COLLECT\_LIST](/intl.en-US/Development/SQL/Builtin functions/Aggregate functions.mdsection_fth_1w1_wdb)|COLLECT LIST|N/A|COLLECT|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[COLLECT\_SET](/intl.en-US/Development/SQL/Builtin functions/Aggregate functions.md)|COLLECT SET|N/A|COLLECT|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[VARIANCE/VAR\_POP](/intl.en-US/Development/SQL/Builtin functions/Aggregate functions.mdsection_emm_lw1_wdb)|VARIANCE/VAR\_POP|VAR\_POP|VARIANCE/VAR\_POP|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[VAR\_SAMP](/intl.en-US/Development/SQL/Builtin functions/Aggregate functions.mdsection_dt5_tw1_wdb)|VAR\_SAMP|VAR\_SAMP|VAR\_SAMP|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[COVAR\_POP](/intl.en-US/Development/SQL/Builtin functions/Aggregate functions.mdsection_hng_1x1_wdb)|COVAR\_POP|N/A|COVAR\_POP|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[COVAR\_SAMP](/intl.en-US/Development/SQL/Builtin functions/Aggregate functions.md)|COVAR\_SAMP|N/A|COVAR\_SAMP|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[PERCENTILE](/intl.en-US/Development/SQL/Builtin functions/Aggregate functions.mdsection_lqv_lx1_wdb)|PERCENTILE|N/A|N/A|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[String functions](/intl.en-US/Development/SQL/Builtin functions/String functions.md)|[CHAR\_MATCHCOUNT](/intl.en-US/Development/SQL/Builtin functions/String functions.md)|N/A|N/A|N/A|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[CHR](/intl.en-US/Development/SQL/Builtin functions/String functions.mdsection_s5r_lwz_vdb)|CHR|CHAR|CHR|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[CONCAT](/intl.en-US/Development/SQL/Builtin functions/String functions.mdsection_xxj_wwz_vdb)|CONCAT|CONCAT|CONCAT|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[GET\_JSON\_OBJECT](/intl.en-US/Development/SQL/Builtin functions/String functions.md)|GET\_JSON\_OBJECT|JSON\_EXTRACT\(\)|N/A|-   Supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[INSTR](/intl.en-US/Development/SQL/Builtin functions/String functions.md)|INSTR|INSTR|INSTR|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[IS\_ENCODING](/intl.en-US/Development/SQL/Builtin functions/String functions.md)|N/A|N/A|N/A|-   Supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[KEYVALUE](/intl.en-US/Development/SQL/Builtin functions/String functions.md)|N/A|N/A|N/A|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[LENGTH](/intl.en-US/Development/SQL/Builtin functions/String functions.md)|LENGTH|LENGTH|LENGTH|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[LENGTHB](/intl.en-US/Development/SQL/Builtin functions/String functions.mdsection_o3y_pzz_vdb)|LENGTHB|LENGTHB|LENGTHB|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[MD5](/intl.en-US/Development/SQL/Builtin functions/String functions.md)|MD5|MD5|N/A|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[REGEXP\_EXTRACT](/intl.en-US/Development/SQL/Builtin functions/String functions.md)|REGEXP\_EXTRACT|N/A|N/A|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[REGEXP\_INSTR](/intl.en-US/Development/SQL/Builtin functions/String functions.mdsection_jpn_5c1_wdb)|N/A|REGEXP\_INSTR|REGEXP\_INSTR|-   Supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[REGEXP\_REPLACE](/intl.en-US/Development/SQL/Builtin functions/String functions.md)|REGEXP\_REPLACE|REGEXP\_REPLACE|REGEXP\_REPLACE|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[REGEXP\_SUBSTR](/intl.en-US/Development/SQL/Builtin functions/String functions.mdsection_k5b_qd1_wdb)|N/A|REGEXP\_SUBSTR|REGEXP\_SUBSTR|-   Supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[REGEXP\_COUNT](/intl.en-US/Development/SQL/Builtin functions/String functions.md)|N/A|N/A|REGEXP\_COUNT|-   Supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[SPLIT\_PART](/intl.en-US/Development/SQL/Builtin functions/String functions.md)|N/A|N/A|N/A|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[SUBSTR](/intl.en-US/Development/SQL/Builtin functions/String functions.mdsection_nkj_1f1_wdb)|SUBSTR|SUBSTR|SUBSTR|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[SUBSTRING](/intl.en-US/Development/SQL/Builtin functions/String functions.md)|SUBSTRING|SUBSTRING|SUBSTR|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[TOLOWER](/intl.en-US/Development/SQL/Builtin functions/String functions.md)|LOWER|LOWER|LOWER|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[TOUPPER](/intl.en-US/Development/SQL/Builtin functions/String functions.mdsection_qvg_sg1_wdb)|UPPER|UPPER|UPPER|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[TRIM](/intl.en-US/Development/SQL/Builtin functions/String functions.mdsection_mf1_3h1_wdb)|TRIM|TRIM|TRIM|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[LTRIM](/intl.en-US/Development/SQL/Builtin functions/String functions.md)|LTRIM|LTRIM|LTRIM|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[RTRIM](/intl.en-US/Development/SQL/Builtin functions/String functions.mdsection_gtk_rh1_wdb)|RTRIM|RTRIM|LTRIM|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[REVERSE](/intl.en-US/Development/SQL/Builtin functions/String functions.mdsection_or4_d31_wdb)|REVERSE|REVERSE|REVERSE|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|SPACE|SPACE|SPACE|SPACE|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[REPEAT](/intl.en-US/Development/SQL/Builtin functions/String functions.mdsection_gnq_m31_wdb)|REPEAT|REPEAT|REPEAT|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[ASCII](/intl.en-US/Development/SQL/Builtin functions/String functions.md)|ASCII|ASCII|ASCII|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[CONCAT\_WS](/intl.en-US/Development/SQL/Builtin functions/String functions.mdsection_xnf_sj1_wdb)|CONCAT\_WS|CONCAT\_WS|N/A|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[LPAD](/intl.en-US/Development/SQL/Builtin functions/String functions.md)|LPAD|LPAD|LPAD|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[RPAD](/intl.en-US/Development/SQL/Builtin functions/String functions.md)|RPAD|RPAD|RPAD|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[REPLACE](/intl.en-US/Development/SQL/Builtin functions/String functions.md)|N/A|REPLACE|REPLACE|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[SOUNDEX](/intl.en-US/Development/SQL/Builtin functions/String functions.md)|SOUNDEX|SOUNDEX|SOUNDEX|-   Supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[SUBSTRING\_INDEX](/intl.en-US/Development/SQL/Builtin functions/String functions.mdsection_uw3_hl1_wdb)|SUBSTRING\_INDEX|SUBSTRING\_INDEX|N/A|-   Not supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[TRANSLATE](/intl.en-US/Development/SQL/Builtin functions/String functions.mdsection_bk1_nl1_wdb)|TRANSLATE|N/A|TRANSLATE|-   Not supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[URL\_DECODE](/intl.en-US/Development/SQL/Builtin functions/String functions.md)|N/A|N/A|N/A|-   Supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[URL\_ENCODE](/intl.en-US/Development/SQL/Builtin functions/String functions.md)|N/A|N/A|PERCENTILE\_CONT|-   Supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|CRC32|CRC32|CRC32|N/A|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[Other functions](/intl.en-US/Development/SQL/Builtin functions/Other functions.md)|[CAST](/intl.en-US/Development/SQL/Builtin functions/Other functions.mdsection_bpc_dy1_wdb)|CAST|CAST|CAST|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[COALESCE](/intl.en-US/Development/SQL/Builtin functions/Other functions.md)|COALESCE|COALESCE|COALESCE|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[DECODE](/intl.en-US/Development/SQL/Builtin functions/Other functions.mdsection_ygq_4y1_wdb)|DECODE|N/A|DECODE|-   Supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[GET\_IDCARD\_AGE](/intl.en-US/Development/SQL/Builtin functions/Other functions.md)|N/A|N/A|N/A|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[GET\_IDCARD\_BIRTHDAY](/intl.en-US/Development/SQL/Builtin functions/Other functions.md)|N/A|N/A|N/A|-   Supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[GET\_IDCARD\_SEX](/intl.en-US/Development/SQL/Builtin functions/Other functions.mdsection_akt_gz1_wdb)|N/A|N/A|N/A|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[GREATEST](/intl.en-US/Development/SQL/Builtin functions/Other functions.mdsection_n1g_kz1_wdb)|GREATEST|GREATEST|N/A|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[ORDINAL](/intl.en-US/Development/SQL/Builtin functions/Other functions.mdsection_pcj_pz1_wdb)|N/A|N/A|N/A|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[LEAST](/intl.en-US/Development/SQL/Builtin functions/Other functions.mdsection_m1b_xz1_wdb)|LEAST|LEAST|LEAST|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[MAX\_PT](/intl.en-US/Development/SQL/Builtin functions/Other functions.mdsection_yyf_d1b_wdb)|N/A|N/A|N/A|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[UUID](/intl.en-US/Development/SQL/Builtin functions/Other functions.mdsection_gtg_j1b_wdb)|N/A|UUID|UID|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[SAMPLE](/intl.en-US/Development/SQL/Builtin functions/Other functions.mdsection_vvl_l1b_wdb)|N/A|N/A|N/A|-   Supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[IF](/intl.en-US/Development/SQL/Builtin functions/Other functions.mdsection_mg5_1bb_wdb)|IF|IF|IF|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[CASE WHEN](/intl.en-US/Development/SQL/Builtin functions/Other functions.mdsection_axm_v1b_wdb)|CASE WHEN|CASE WHEN|CASE WHEN|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[SPLIT](/intl.en-US/Development/SQL/Builtin functions/Other functions.md)|SPLIT|SPLIT|N/A|-   Supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[STR\_TO\_MAP](/intl.en-US/Development/SQL/Builtin functions/Other functions.mdsection_p1z_xrj_dfb)|STR\_TO\_MAP|N/A|N/A|-   Supported in MaxCompute mode
-   Not supported in Hive-compatible mode |
|[EXPLODE](/intl.en-US/Development/SQL/Builtin functions/Other functions.md)|EXPLODE|N/A|N/A|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[MAP](/intl.en-US/Development/SQL/Builtin functions/Other functions.mdsection_bzn_hcb_wdb)|MAP|N/A|N/A|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[MAP\_KEYS](/intl.en-US/Development/SQL/Builtin functions/Other functions.md)|MAP\_KEYS|N/A|N/A|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[MAP\_VALUES](/intl.en-US/Development/SQL/Builtin functions/Other functions.mdsection_pd3_jlb_wdb)|MAP\_VALUES|N/A|N/A|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[NVL](/intl.en-US/Development/SQL/Builtin functions/Other functions.mdsection_c2j_hbb_wdb)|NVL|IFNULL|N/A|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[ARRAY](/intl.en-US/Development/SQL/Builtin functions/Other functions.mdsection_zcz_4lb_wdb)|ARRAY|N/A|N/A|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[SIZE](/intl.en-US/Development/SQL/Builtin functions/Other functions.mdsection_nr5_vlb_wdb)|SIZE|N/A|N/A|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[ARRAY\_CONTAINS](/intl.en-US/Development/SQL/Builtin functions/Other functions.mdsection_rk2_2mb_wdb)|ARRAY\_CONTAINS|N/A|N/A|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[POSEXPLODE](/intl.en-US/Development/SQL/Builtin functions/Other functions.mdsection_ilc_xmb_wdb)|POSEXPLODE|N/A|N/A|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[TRANS\_ARRAY](/intl.en-US/Development/SQL/Builtin functions/Other functions.md)|N/A|N/A|N/A|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[INLINE](/intl.en-US/Development/SQL/Builtin functions/Other functions.mdsection_w3l_pnb_wdb)|INLINE|N/A|N/A|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |
|[NAMED\_STRUCT](/intl.en-US/Development/SQL/Builtin functions/Other functions.md)|N/A|N/A|N/A|-   Supported in MaxCompute mode
-   Supported in Hive-compatible mode |

**Note:** The MaxCompute mode is enabled by default. To use the Hive-compatible mode, run the following commands:

```
-- Switch to the Hive-compatible mode at the project level.
setproject odps.sql.hive.compatible=True;
-- Switch to the Hive-compatible mode at the session level.
set odps.sql.hive.compatible=True;
```

