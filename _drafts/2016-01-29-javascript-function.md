javascript 内建函数

JavaScript splice() 方法

splice() 方法用于插入、删除或替换数组的元素。
注意：这种方法会改变原始数组！。


returnArray array.splice(index,howmany,item1,.....,itemX)


|参数|描述|
|----|------|
|index|必需。规定从何处添加/删除元素。该参数是开始插入和（或）删除的数组元素的下标，必须是数字。|
|howmany|必需。规定应该删除多少元素。必须是数字，但可以是 "0"。如果未规定此参数，则删除从 index 开始到原数组结尾的所有元素。|
|item1, ..., itemX|可选。要添加到数组的新元素|
|returnArray|如果从 arrayObject 中删除了元素，则返回的是含有被删除的元素的数组。|