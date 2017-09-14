title: java SQL动态参数绑定工具
date: 2017-08-7 00:00:00
tags: [javaUtil]

---

- SqlAutoParamters.java
```
import java.util.LinkedList;
import java.util.Queue;

/**
* @author liuxiang on 2017/8/7.
*/
public class SqlAutoParamters {

    public static void main(String[] args) throws NoSuchMethodException {

        // TODO 待java多行字符串方法优化,去除繁杂的字符串加工
        String autoParameters = "" +
                "[2017-08-07 12:25:21]  DEBUG queryByPage - ooo Using Connection [com.alibaba.druid.proxy.jdbc.ConnectionProxyImpl@66af0e39]\n" +
                "[2017-08-07 12:25:21]  DEBUG queryByPage - ==>  Preparing: SELECT partner_code, balance_ptg, avail_day, daily_peak FROM warning_config WHERE is_deleted=0 AND `partner_code` IN (?,?,?,?,?,?) ORDER BY gmt_modify DESC LIMIT ?, ? \n" +
                "[2017-08-07 12:25:21]  DEBUG queryByPage - ==> Parameters: 111111(String), 1111(String), 0202(String), 0125_com(String), 11111111111111scUf(String), all(String), 0(Integer), 10(Integer)\n" +
                "[2017-08-07 12:25:21]  DEBUG queryByPage - <==      Total: 4\n" +
                "[2017-08-07 12:25:21]  DEBUG countByPage - ooo Using Connection [com.alibaba.druid.proxy.jdbc.ConnectionProxyImpl@66af0e39]\n" +
                "[2017-08-07 12:25:21]  DEBUG countByPage - ==>  Preparing: SELECT count(*) FROM warning_config WHERE is_deleted=0 AND `partner_code` IN (?,?,?,?,?,?) \n" +
                "[2017-08-07 12:25:21]  DEBUG countByPage - ==> Parameters: 111111(String), 1111(String), 0202(String), 0125_com(String), 11111111111111scUf(String), all(String)\n" +
                "[2017-08-07 12:25:21]  DEBUG countByPage - <==      Total: 1";
        // System.out.println(autoParameters);

        // 多sql
        showSqlAll(autoParameters);

        // 单sql
//        String sql = autoParaminding(autoParameters);
//        System.out.println(sql);
    }

    static void showSqlAll(String autoParameters) {
        String[] aps = autoParameters.split("<==      Total");
        for (int i = 0; i < aps.length - 1; i++) {
            String sql = autoParaminding(aps[i]);
            System.out.println(sql);
        }
    }

    static String autoParaminding(String autoParameters) {

        int preparingBegin = autoParameters.indexOf("Preparing: ") + 11;
        int preparingend = autoParameters.indexOf("\n", preparingBegin);
        String preparing = autoParameters.substring(preparingBegin, preparingend);
//        System.out.println(preparing);// 动态参数Sql

        int parametersBegin = autoParameters.indexOf("Parameters: ") + 12;
        int parametersEnd = autoParameters.indexOf("\n", parametersBegin);
        String parameters = autoParameters.substring(parametersBegin, parametersEnd);
//        System.out.println(parameters);// 动态参数

        return showSql(preparing, parameters);// show
    }

    static String showSql(String preparing, String parameters) {
        String[] ps = parameters.split(",");
        int i = 0;
        while (preparing.indexOf("?") != -1) {

//            System.out.println(ps[i]);
            String p = ps[i].split("\\(")[0].trim();
            if (ps[i].contains("String")) {
                preparing = preparing.replaceFirst("\\?", "'" + p + "'");// 字符串补单引号
            } else {
                preparing = preparing.replaceFirst("\\?", p);
            }
            i++;
        }

//        System.out.println(preparing);
        return preparing + ";";
    }

    /**
    * 拆解具体参数值(丢失了数据类型,暂不好用)
    *
    * @param parameters
    * @return
    */
    static Queue<String> parameter2List(String parameters) {
        String[] ps = parameters.split(",");
        Queue<String> queue = new LinkedList<String>();
        for (String str : ps) {
            String p = str.split("\\(")[0];
            queue.offer(p);//追加元素
        }
        return queue;
    }
}

/**
* [2017-08-07 10:10:00]  DEBUG queryByPage - ==>  Preparing: SELECT `flow_account`.* FROM `flow_account` LEFT JOIN `service` ON `flow_account`.`service`=`service`.`name` WHERE `flow_account`.`partner_code` IN (?,?,?,?,?) AND `flow_account`.`deleted`=0 ORDER BY `flow_account`.`gmt_begin` DESC LIMIT ?, ?
* [2017-08-07 10:10:00]  DEBUG queryByPage - ==> Parameters: 11111111111111scUf(String), 0202(String), 0125_com(String), 111111(String), 1111(String), 0(Integer), 10(Integer)
* [2017-08-07 10:10:00]  DEBUG queryByPage - <==      Total: 7
* <p>
* [2017-08-07 11:31:50]  DEBUG queryByPage - ooo Using Connection [com.alibaba.druid.proxy.jdbc.ConnectionProxyImpl@aba6537]
* [2017-08-07 11:31:50]  DEBUG queryByPage - ==>  Preparing: SELECT `contract`.* FROM `contract` where `contract`.`is_deleted`='N' order BY `contract`.`gmt_create` DESC LIMIT ?, ?
* [2017-08-07 11:31:50]  DEBUG queryByPage - ==> Parameters: 0(Integer), 10(Integer)
* [2017-08-07 11:31:50]  DEBUG queryByPage - <==      Total: 10
* [2017-08-07 11:31:50]  DEBUG Timer - [billing]  操作计时：155ms cn.fraudmetrix.billing.dubbo.OaManagerImpl.getContractByPage(..) result:[cn.fraudmetrix.billing.client.oa.object.ContractRecord@7880205, cn.fraudmetrix.billing.client.oa.object.ContractRecord@58df654b, cn.fraudmetrix.billing.client.oa.object.ContractRecord@36456296, cn.fraudmetrix.billing.client.oa.object.ContractRecord@20f85b7a, cn.fraudmetrix.billing.client.oa.object.ContractRecord@67a7fd4a, cn.fraudmetrix.billing.client.oa.object.ContractRecord@44f139c8, cn.fraudmetrix.billing.client.oa.object.ContractRecord@59bda212, cn.fraudmetrix.billing.client.oa.object.ContractRecord@3bfc11d4, cn.fraudmetrix.billing.client.oa.object.ContractRecord@63e9e64c, cn.fraudmetrix.billing.client.oa.object.ContractRecord@2e9bbc80]
* [2017-08-07 11:31:50]  DEBUG countByPage - ooo Using Connection [com.alibaba.druid.proxy.jdbc.ConnectionProxyImpl@aba6537]
* [2017-08-07 11:31:50]  DEBUG countByPage - ==>  Preparing: SELECT COUNT(*) FROM `contract` WHERE `contract`.`is_deleted`='N'
* [2017-08-07 11:31:50]  DEBUG countByPage - ==> Parameters:
* [2017-08-07 11:31:50]  DEBUG countByPage - <==      Total: 1
*/
```
