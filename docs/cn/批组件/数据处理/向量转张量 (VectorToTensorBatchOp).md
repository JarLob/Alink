# 向量转张量 (VectorToTensorBatchOp)
Java 类名：com.alibaba.alink.operator.batch.dataproc.VectorToTensorBatchOp

Python 类名：VectorToTensorBatchOp


## 功能介绍
转换向量类型为张量类型。

## 参数说明

| 名称 | 中文名称 | 描述 | 类型 | 是否必须？ | 默认值 |
| --- | --- | --- | --- | --- | --- |
| selectedCol | 选中的列名 | 计算列对应的列名 | String | ✓ |  |
| handleInvalidMethod | 处理无效值的方法 | 处理无效值的方法，可取 error, skip | String |  | "ERROR" |
| outputCol | 输出结果列 | 输出结果列列名，可选，默认null | String |  | null |
| reservedCols | 算法保留列名 | 算法保留列 | String[] |  | null |
| tensorDataType | 要转换的张量数据类型 | 要转换的张量数据类型。 | String |  |  |
| tensorShape | 张量形状 | 张量的形状，数组类型。 | Long[] |  | null |
| numThreads | 组件多线程线程个数 | 组件多线程线程个数 | Integer |  | 1 |


## 代码示例
### Python 代码
```python
from pyalink.alink import *

import pandas as pd

useLocalEnv(1)

df_data = pd.DataFrame([
    ['0.0 0.1 1.0 1.1 2.0 2.1']
])

batch_data = BatchOperator.fromDataframe(df_data, schemaStr = 'vec string')

batch_data.link(VectorToTensorBatchOp().setSelectedCol("vec")).print()

```
### Java 代码
```java
import org.apache.flink.types.Row;

import com.alibaba.alink.operator.batch.source.MemSourceBatchOp;
import org.junit.Test;

import java.util.Collections;
import java.util.List;

public class VectorToTensorBatchOpTest {

	@Test
	public void testVectorToTensorStreamOp() throws Exception {
		List <Row> data = Collections.singletonList(Row.of("0.0 0.1 1.0 1.1 2.0 2.1"));

		MemSourceBatchOp memSourceBatchOp = new MemSourceBatchOp(data, "vec string");

		memSourceBatchOp
			.link(
				new VectorToTensorBatchOp()
					.setSelectedCol("vec")
			)
			.print();
	}
}
```

### 运行结果

| vec                              |
|----------------------------------|
| DOUBLE#6#0.0 0.1 1.0 1.1 2.0 2.1 |
