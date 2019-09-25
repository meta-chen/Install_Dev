# OWL学习

### 属性基数限制（*Property Cardinality Restrictions*）

指定被约束的个体数量（ specify the number of individuals involved in the restriction）

声明一个类实例：John最多有4个有自己孩子的孩子

```
ClassAssertion(
   ObjectMaxCardinality( 4 :hasChild :Parent ) 
   :John
 )
```

```
ObjectMaxCardinality 限制上限
ObjectMinCardinality 限制下限
ObjectExactCardinality 明确数量
```



### 枚举个体对象（*Enumeration of Individuals*）

列出一个类的所有实例（describe a class is just to enumerate all its instances）

```
 EquivalentClasses(
   :MyBirthdayGuests
   ObjectOneOf( :Bill :John :Mary)
 )
```

Note: 这种事实（axiom，英文为公理，但是我觉得在这个语境下叫事实更好 ）描述提供的信息比以下方式多，而且上一种方式确定了 MyBirthdayGuests 中有且仅有的人，再声明如下断言只能说明Bob是上面3人中一人：

```
classAssertion( :MyBirthdayGuests  :Bob)
```



### 其他属性

- 可逆属性（*Inverse*）：

```
InverseObjectProperties( :hasParent :hasChild ) 
```

可用如下语句直接表示 *:hasParent*

```
ObjectInverseOf( :hasChild )
```

因此可以有：声明一个 *Orphan*类

```
 EquivalentClasses(
   :Orphan
   ObjectAllValuesFrom(
     ObjectInverseOf( :hasChild )
     :Dead
   )
 ) 
```

- 对称属性（*symmetric*）: *A有配偶B* 意味着 *B也有配偶A*

```
SymmetricObjectProperty( :hasSpouse ) 
```

- 非对称属性（*asymmetric*）：A可以到B，但是B不能到A

```
 AsymmetricObjectProperty( :hasChild ) 
```

Note：*asymmetric*概念比*non-symmetric*强

- 不相交属性（*Disjoint*）：两个属性不会同时出现在两个个体间

```
DisjointObjectProperties( :hasParent :hasSpouse )  
```

- 自反属性（*reflexive*）：个体与自己间具有的属性

```
 ReflexiveObjectProperty( :hasRelative )
```

每个人都可以做自己的亲属（？听起来怪怪的，但是仔细想也有道理，主要是这个例子是w3c提供的，哈哈）

- 非自反属性（Irreflexive）：个体与自己间不可能具有的属性

```
IrreflexiveObjectProperty( :parentOf )   
```

- 函数型属性（*Functional*)：一对一关系

```
FunctionalObjectProperty( :hasHusband ) 
```

Note：姑且认为命题成立，w3c上特殊标注>_<。这个属性并不要求每个个体必须有一个Husband，只是限制了最多有一个。

- 反函数型属性（*InverseFunctional*）

```
 InverseFunctionalObjectProperty( :hasHusband ) 
```

Note：一个个体最多是一个人的丈夫，但是也可以看出函数型和反函数型的差别，在一夫多妻制下，前者成立，而后者并不。

- 传递性属性（*Transitive*）

```
 TransitiveObjectProperty( :hasAncestor )
```



### 属性链（Property Chains）

```
SubObjectPropertyOf( 
   ObjectPropertyChain( :hasParent :hasParent ) 
   :hasGrandparent 
 )
```

:hasParent 是一个传递属性，两个:hasParent组合成的属性链可以得到 :hasGrandparent 



### 键（Keys）

指定一系列的数据或对象属性可以作为一个类的键（*a collection of (data or object) properties can be assigned as a key to a class expression*）

```
 HasKey( :Person () ( :hasSSN ) )
```