Learning LeanCloud
===

## Python
之前删除对象时，忘记把有关的引用此对象为 pointer 的对象删除。
再次查询删除时，需要判断这个 pointer 是否存在，发现情况如下：

* 查询时是否使用`include` 的区别：
```python
import leancloud


Sku = leancloud.Object.extend('Sku')
History = leancloud.Object.extend('History')

sku = Sku()
sku.set('name', 'Some name')
sku.save()

history = History()
history.set('sku', sku)  # 创建 Pointer
history.save()

# 删除数据，但未删除 Pointer 的引用。
sku.detroy()

# 查找数据，判断哪一行的 Pointer 没有引用。
# 在 History 表中，sku 字段还是有值的，值就是被删除的 sku 的 objectId。
# 但这个 sku 指向的 Pointer 已经被删除。

# 不使用 include。
results = History.query.find()
results[0].get('sku')  # Out: <leancloud.object_.Sku at 0x58171d0>
results[0].get('sku').dump()  # Out: {'objectId': '59be81778d6d810055a2cfab'}
results[0].get('sku')._dump()   # Out: 
                                # {'__type': 'Object',
                                #  'className': 'Sku',
                                #  'objectId': '59be81778d6d810055a2cfab'}

# 使用 include。
results = History.query.include('sku').find()
results[0].get('sku')  # Out: 
print(results[0].get('sku'))  # Out: None

```
