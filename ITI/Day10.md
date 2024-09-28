# Linq Day 2
having `productlist`
`var result =` return type is sequence that implementing IEumerable interface 
basicly determined acording to query operator you use
## `Where`

Filteration function using to filter sequence based on bool func
**it has two overloading**
- First
  - `first input ienumerable`
  - `second input func -> take object and return bool `
```cs
var result = productlist.where(p=>p.unitsinstock==0);
    result = form p in productlist 
                where p.unitinstock==0
                select p;

result = productlist.where( p=> (p.unitsinstock==0) && (p.category=="meat"));

```
- Second  indexed where
  - `valid only on fluent syntax format`
  - `first input ienumerable`
  - `second input func -> take object and index and return bool `
```cs
var result = productlist.where((p,i)=>p.unitsinstock==0 && i<=10);
```

## `Select`
Transformation operator -> transform every element in i/p seq to a new element in o/p seq of new data type
`num of i/p seq = num o\p seq`
for ex : want to return new obj (name , id) only `anynoums type`

```cs
    result =productlist.select(p=>p.productname);
            = from p in productlist
                select p.productname;
    // old object (product) -> new (string)

    var res = productlist.select(p=>new{ Id=p.productid , p.productname})
        // return is ienumerable of anynoumes type
        res = from productlist
                select new {productid,productname}


    var DiscountedProduct=productlist.select(p=>new product() {
        productname,
        productid,
        unitprice=0.9M*p.unitprice
    })
```
**using select and where**
it's an example of progressive which means chaining the method as you like
```cs
    var Result=ProductList.Select(P => new { Name P.ProductName, NewPrice = P.UnitPrice * 1.1M }).Where (P => P.NewPrice > 20);

    //select must be last query so must make it two query 
    //one make new object 
    //second search in the new object after we modify it
    var RR01 = from Pin ProductList
                select new { Name = P.ProductName, NewPrice = P.UnitPrice * 1.1M };
    var RR02 = from Pin RR01
                where P.NewPrice > 20
                select P;
    //to make it in one query
    //using into
    var RR =from Pin ProductList
            select new { Name P.ProductName, NewPrice P.UnitPrice * 1.1M }
            into TaxedPrd /// Introduct new Range Variable to Continue Query using New(Only) Range Variable
            where TaxedPrd.NewPrice > 20
            select TaxedPrd;
```
 
**Second indexed select**
`valid only on fluent syntax format`
```cs
//index is order of object in the list 5 element [0-4]
res = productlist.select((p,i)=>new{ index=i , p.productname})
```


## `Ordering Elements`
##### Ordering by one property 
`OrderBy`
```cs
    result = productlist.OrderBy( p => p.unitsinstock );

    result = from p in list
                orderby p.unitsinstock
                select p;
```
`OrderByDescending`

```cs
    result = productlist.OrderByDescending( p => p.unitsinstock );

    result = from p in list
                orderby p.unitsinstock descending
                select p;
```
##### Ordering by more one element 

```cs
    result = productlist.OrderByDescending( p => p.unitsinstock )
                        .thenby(p=>p.unitprice); //order with price asending
    result = productlist.OrderByDescending( p => p.unitsinstock )
                        .thenbyDescinding(p=>p.unitprice); //order with price asending

    result = from p in list
                orderby p.unitsinstock descending , p.unitprice
                select p;

    result = from p in list
                orderby p.unitsinstock descending , p.unitprice descinding
                select p;
```



## `Element Operators - Imidiate Execution`
```cs
var Result = productlist.First();
    result = productlist.First();
    Result = ProductList.Last();
    Result = ProductList. First (P => P. UnitsInStock == 0);
    Result = ProductList. Last(P => P. UnitsInStock == 0);
Console.WriteLine (Result?. ProductName ?? "NA");
```
**if list is empty**
```cs

list<product> lst =new list<product>();
//if the inp seq has no elements , throw exception
var result = lst.First();
    result = lst.last();

// to solve it 
result = lst.FirstOrDefault();
result = lst.LastOrDefault();
//this eq to result = default (Product)
Console.WriteLine (Result?. ProductName ?? "NA");
```
**has another overload that take condition**
```cs
var result = ProductList.First(p=>p.unitPrice>300);//this list can be has elements and throw exception beacuse no elements match the condition
var result = ProductList.FirstOrDefault(p=>p.unitPrice>300);
```
`so using OrDefault is always the best no to throw exception`

`this elements operators not valid in quary exceprssion we can use hybrid syntax`

`(Query Expression).Fluent Syntax`
```cs
var R = (from Pin ProductList
        where P.UnitsInStock == 0
        select new { P.ProductName, P. Category }). First();
```

```cs
    res = productlist.ElementAt(170);//Exception
    res = productlist.ElementAtOrDefault(170);
```

```cs
    //DiscountedList is empty
    DiscountedList.Add(ProductList[0])

    res = productlist.Single();//excption if not one element,if zero or 2 or more 
    res = DiscountedList.Single();//return the one element
```
```cs
    res = productlist.Single(P=>P.ProductId==7);
    //throw exception if there is more one match predicate or no one match
    res = productlist.SingleOrDefault(P=>P.ProductId==7);
    //exception if more one match , no exception if zero
    Console.WriteLine (Result?. ProductName ?? "NA");
```

##  `Aggregate - Imidiate Execution`
min , max , count
**Selector** `func that take obj and return value`
```cs
var result = ProductList.Count;//built in prop of list 
    result = ProductList.Count();//extension method of linq no diff between two

    result = ProductList.Count(P=> P.UnitInStock==0);
```
```cs
    //return max product based on IComparable 
    //if no icomparable it will throw Exception
    var result = ProductList.Max(); ///this will return object with max according to compareTo

    //the func will take odject and return the number value
    result = ProductList.Max( p => p.UnitPrice);/// return value
```
```cs
    var result = ProductList.Min(); ///this will return object with max according to compareTo
    //the func will take odject and return the number value
    result = ProductList.Min( p => p.UnitPrice);/// return value
```
```cs
    result = ProductList.Average( p => p.UnitPrice);/// return value
```
```cs
    result = ProductList.Sum( p => p.UnitPrice);/// return value
```
search for it `ProductList.Aggregate`

## `Generators Operators`
**generating output seq**
**only way to call them is as static memberes from Enumerable class**
```cs
var res = Enumerable.Range(0,10);//only integer numbers
//take two number start and count not end
var r1 = Enumerable.Empty<product>();
//must determind T type

var r2 = Enumerable.Reapeat(3,10);
//repeat number (3) 10 time take care that make boxing 10 time
var r3 = Enumerable.Reapeat(ProductList[2],5);
//this make copy of ref
//so when change one product all list in r3 is the same
```
## `Select Many`
**Output Seq count > Input Seq Count
transform each element in input Seq to subSeq (IEnumerable<>)**

function make in select many is any function that produce list 
like  `Split` , `ToCharArray` ...
```cs
List<String> NameList = new List<String>(){
    "Ahmed Ahmed","Ali Ali","Mohamed Mohamed"
}
var result = NameList.SelectMany(fn = > fn.Split(' '));
//result will be list of 6 names first will split to 2 obj
//each will be and make all list in one list 
```

## `Casting Operators : Imidiate Execution`
```cs
//List<Product> lst = ProductList.Where(p=>p.UnitsInStock==0); 
//1. here is wrong
//2. if we write var it will result in decorator not actually data
//3. toList make copy of data imidiataly not decorator

List<Product> pList = ProductList.Where(p=>p.UnitsInStock==0).ToList();

Product[] pArr = ProductList.Where(p=>p.UnitsInStock==0).ToArray();

var lst = ProductList.Where(p=>p.UnitsInStock==0)
//.ToHashSet();//must obj declare the equal m gethashcode
//.ToHashSet(Comperar);comparar is object from class that implement IEqualityComparer that has gethashcode and equality
//.ToDictionary(p=>p.productId);//choose the KeySelector result Dic<long,product>
//.ToDictionary(p=>p.productId,prd=>new{prd.productid,prd.productName});//choose the keySelector,and valueSelector result Dic<long,anonymous>
//.ToLookUp();
```

## `Set Operators`

```cs
var seq1 = Enumerable.Range(0,99); // 0:99
var seq2 = Enumerable.Range(50,100); // 50:149 

var res = seq1.Union(seq2);//remove the duplicates
//if no comparerer defined , will take IEqualityComparer obj

var res = seq1.Concate(seq2);//allow duplicates

res = seq1.Distinct(); //also take comparer object

res = seq1.Except(seq2);//in seq1 and not in seq2
res = seq1.Intersect(seq2);//in seq1 and in seq2
```
## `Quantifiers , return Boolean`
`any`
```cs
Console.Write(ProductList.Any());//return true if seq have at least one element
Console.Write(ProductList.Any(p=>p.unitprice>200));//can take predicate
```
`all`
```cs
Console.Write(ProductList.All(p=>p.unitprice>200));// return true if all elements match predicate
```
`SeqenceEqual`
```cs
Console.Write(seq1.SeqenceEqual(seq2));// return true if two equal using default IEqualityComparer
Console.Write(seq1.SeqenceEqual(seq2,Comparer));//also can take IEqualityComparer Object
```
## `Zip`
`input 2 seq , one combined seq`

```cs
int[] numbers = { 3, 5, 7 };
string[] words = { "three", "five", "seven", "ignored" };
IEnumerable<string> zip = words.Zip (numbers, (w, n) => w + "=" + n);// compine two object in one 
//three = 3
//five = 5 
//seven = 7
///result will be same size like smaller to can combine the two seq 
```
## `Grouping`
`result in groups not one seq (look like many seqences)`
```cs
var res = from p in productlist
            where p.unitsinstock > 0
            group p by p.category;

//return IEnumerable of igroup<object>
foreach (var PrdGroup in Result)
{
    Console.WriteLine($"Group Key {PrdGroup.Key}"); //each group has key
    foreach (var Prd in PrdGroup) 
    {
        Console.WriteLine($".. {Prd.ProductName}");
    }
}   
//can continue the query after grouping
var res = from p in productlist
            where p.unitsinstock > 0
            group p by p.category 
            into PrdGroup                   // making new seq to can make query on it
            where PrdGroup.Count() > 5
            orderby PrdGroup.Count() descending
            //select PrdGroup;
            select new{Category = PrdGroup.Key , ProductCount = PrdGroup.Count()};
```
`here the query we can consider it easier than fluent expression`
```cs
var res = ProductList.GroupBy(p=>p.productcategory).where(prdG=>prdG.Count()>5)
                    .orderByDescending(PG=>PG.Count())
                    .Select(PG=>new{Category = PrdGroup.Key , ProductCount = PrdGroup.Count()};)
```
## `Let , Into`
`Introductin new range variable in quary syntax`
`introduce new variables or restart the query again`

```cs
List<string> Names = new List<string>(){ "Ahmed" "Aly" "Mai" "Sally" "Moemen" "Shimaa"};
///aoieuAOIEU

//Reqular Expression -> class Regex  has two fun match ,replace

var Result = from n in Names
             select Regex.Replace(n,"[aoieuAOIEU]",String.Empty)//here we want to make where on new result
             //need to restert the query with new Range Variable , old variable is not accessable
             into NoVol
             where NoVol.length >= 3
             orderby NoVol, NoVol.length() descending // this will order with first char then length
             select NOVol;

var Result = from n in Names
             let NoVol = Regex.Replace(n,"[aoieuAOIEU]",String.Empty)
             //continue the query with added new Range Variable old is accessable
             where NoVol.length >= 3
             orderby NoVol, N.length() descending // using N which is name before modifing the name
             select NOVol;
```
