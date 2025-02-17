# 归一化批预测 (MinMaxScalerPredictBatchOp)
Java 类名：com.alibaba.alink.operator.batch.dataproc.MinMaxScalerPredictBatchOp

Python 类名：MinMaxScalerPredictBatchOp


## 功能介绍

数据归一化预测组件

将数据归一到minValue和maxValue之间，value最终结果为 (value - min) / (max - min) * (maxValue - minValue) + minValue，最终结果的范围为[minValue, maxValue]。

minValue和maxValue由用户指定，默认为0和1。

需要加载由MinMaxScalerTrainBatchOp训练的模型

## 参数说明

| 名称 | 中文名称 | 描述 | 类型 | 是否必须？ | 默认值 |
| --- | --- | --- | --- | --- | --- |
| outputCols | 输出结果列列名数组 | 输出结果列列名数组，可选，默认null | String[] |  | null |
| numThreads | 组件多线程线程个数 | 组件多线程线程个数 | Integer |  | 1 |




## 代码示例
### Python 代码
```python
from pyalink.alink import *

import pandas as pd

useLocalEnv(1)

df = pd.DataFrame([
            ["a", 10.0, 100],
            ["b", -2.5, 9],
            ["c", 100.2, 1],
            ["d", -99.9, 100],
            ["a", 1.4, 1],
            ["b", -2.2, 9],
            ["c", 100.9, 1]
])
             
colnames = ["col1", "col2", "col3"]
selectedColNames = ["col2", "col3"]


inOp = BatchOperator.fromDataframe(df, schemaStr='col1 string, col2 double, col3 long')
         

# train
trainOp = MinMaxScalerTrainBatchOp()\
           .setSelectedCols(selectedColNames)

trainOp.linkFrom(inOp)

# batch predict
predictOp = MinMaxScalerPredictBatchOp()
predictOp.linkFrom(trainOp, inOp).print()


```
### Java 代码
```java
import org.apache.flink.types.Row;

import com.alibaba.alink.operator.batch.BatchOperator;
import com.alibaba.alink.operator.batch.dataproc.MinMaxScalerPredictBatchOp;
import com.alibaba.alink.operator.batch.dataproc.MinMaxScalerTrainBatchOp;
import com.alibaba.alink.operator.batch.source.MemSourceBatchOp;
import org.junit.Test;

import java.util.Arrays;
import java.util.List;

public class MinMaxScalerPredictBatchOpTest {
	@Test
	public void testMinMaxScalerPredictBatchOp() throws Exception {
		List <Row> df = Arrays.asList(
			Row.of("a", 10.0, 100),
			Row.of("b", -2.5, 9),
			Row.of("c", 100.2, 1),
			Row.of("d", -99.9, 100),
			Row.of("a", 1.4, 1),
			Row.of("b", -2.2, 9),
			Row.of("c", 100.9, 1)
		);

		String[] selectedColNames = new String[] {"col2", "col3"};
		BatchOperator <?> inOp = new MemSourceBatchOp(df, "col1 string, col2 double, col3 int");
		BatchOperator <?> trainOp = new MinMaxScalerTrainBatchOp()
			.setSelectedCols(selectedColNames);
		trainOp.linkFrom(inOp);
		BatchOperator <?> predictOp = new MinMaxScalerPredictBatchOp();
		predictOp.linkFrom(trainOp, inOp).print();
	}
}
```

### 运行结果


col1|col2|col3
----|----|----
a|0.5473|1.0000
b|0.4851|0.0808
c|0.9965|0.0000
d|0.0000|1.0000
a|0.5045|0.0000
b|0.4866|0.0808
c|1.0000|0.0000



