title: 抽奖转盘——后端关键逻辑(java)
date: 2016-3-4 00:00:02
categories: javaScript
tags: [javaScript,抽奖转盘]
 
---
# 后端控制 : 抽奖概率 & 库存维护 & 中奖登记
```
package test;
import java.util.Arrays;
import java.util.Random;
public class Test {
    public static void main(String[] args) {
        Object[] stat = { 0, 0, 0, 0 };// 统计
        for (int i = 0; i < 1000; i++) {// 抽取1000次
            int prizedIndex = draw_back();
            stat[prizedIndex] = (int) stat[prizedIndex] + 1;
        }
        System.out.println(Arrays.asList(stat));
    }
    public static int draw_back() {
        synchronized (new Object()) {// 上锁-防并发
            // TODO 数据库获取库存
            Object[][] prizeObj = new Object[][] {
                    /** {id,奖品名,库存(概率)} */
                    { 0, "未中奖", 0 },
                    { 1, "一等奖 iphone", 10 },
                    { 2, "二等奖 书包", 30 },
                    { 3, "三等奖 水杯", 300 }
                    };
            int prizedTotle = 0;
            // 统计总量
            for (Object[] objects : prizeObj) {
                prizedTotle += (int) objects[2];
            }
            /** 未中奖概率控制 */
            {
                prizeObj[0][2] = prizedTotle * 2;// 设置未中奖概率为[2/3]
                prizedTotle += (int) prizeObj[0][2];// 库存增加不中奖
            }
            /** 抽奖(控制概率) */
            {
                int prizedIndex = 0;// 中奖index
                for (int i = 0; i < prizeObj.length; i++) {// 捉个奖项抽取,抽到即停
                    int randomNum = new Random().nextInt(prizedTotle) + 1;// 随机,范围[1~totle]
                    // System.out.println("randomNum:" + randomNum + " " +
                    // prizeObj[i][2]);
                    if (randomNum <= (int) prizeObj[i][2]) {
                        // 中奖
                        prizedIndex = i;
                        break;// 后面的不需要抽了
                    } else {
                        // 未抽中
                        prizedTotle = prizedTotle - (int) prizeObj[i][2]; // 拿走此奖项继续抽
                    }
                }
                System.out.println("中奖:" + prizedIndex + " " + Arrays.asList(prizeObj[prizedIndex]));
                // TODO 用户中奖:更新数据库库存 & 登记中奖纪录
                // 注意:库存递减使用 `update tableName set num = num-1 where id = ?`(目的:数据库管理锁)
                return prizedIndex;
            }
        }
    }
}
```
 
<!-- more -->