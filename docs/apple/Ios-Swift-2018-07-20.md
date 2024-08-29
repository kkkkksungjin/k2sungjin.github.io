---
layout: default
title: Swift Start
parent: IOS
grand_parent: Apple
nav_order: 1
---

# Swift4

소개
<hr/>
swift4기반으로 공부중이며, 처음 공부하는것으로 처음 문법으로 차례대로 정리 해보았습니다. 
공부할때 내용에 대해서는 iBooks > The Swift Programming Language를 참조했습니다.


let 상수 var 변수

~~~java

var 변수 선언

var artistName:String
print("The current value of friendlyWelcome is \(friendlyWelcome)")
// Prints "The current value of friendlyWelcome is Bonjour!’

~~~

playground?

__변수의 타입을 확인해 보자__{: style="color: #e26716"}

~~~java

type(of: 변수)  - 변수 타입을 확인 할때 사용합니다. 

var str = "Hello, playground"
var version = 1.0
let year = 2018
let isData = true

//type annotation
//var str:String = "Hello, playground"
//var version:Double = 1.0
//let year:Int = 2018
//let isData:Bool = true

print("str : \(type(of: str))")
print(str)

print("version : \(type(of: version))")
print(version)

print("year : \(type(of: year))")
print(year)

print("isData : \(type(of: isData))")
print(isData)

---결과---
str : String
Hello, playground
version : Double
1.0
year : Int
2018
isData : Bool
true

~~~

문자열 """ ... """ - 개발자가 입력한 모양대로 그대로 출력하게 만들어줌
~~~java
var testStr = """
테스트를 해
보   까

요
"""

---결과---
테스트를 해
보   까

요
~~~

__문자열 Empty상태인지 확인 해보자__{: style="color: #e26716"}

~~~java

var emptyString = ""
if emptyString.isEmpty {
  print("emptyString: not string")
}

var emptyString2 = " "
if emptyString2.isEmpty {
    print("emptyString2: not string")
}else{
    print("emptyString2: !not string")
}

---결과---
emptyString: not string
emptyString2: !not string

~~~

__배열 만드는법__{: style="color: #e26716"}

~~~java

var someArray = Array<String>()
var someArray2 = [String]()
var somsInt = [Int]()
print(somsInt)
print(someArray)
print(someArray2)

someArray.append("Hello")
print(someArray)

var somsString:[String] = ["K","S","J"]
print(somsString)

var somsString2 = ["KK","Ss","JJ"]
print("\((somsString2)) 길이:[\(somsString2.count)]" )

//배열에 추가 삽입할때 사용것을 확인해보자
somsString2 += ["Hello!!"]
print("\((somsString2)) 길이:[\(somsString2.count)]" )

//배열에 여러개를 한번에 바꿀때 사용것을 확인해보자.
somsString2[1...3] = ["a","b","c"]
print("\((somsString2)) 길이:[\(somsString2.count)]" )

//immutable array
//let로 선언한 배열은 일반 var가변배열 보다 접근속도가 빠르므로 확장되지 않는것은 let:상수로 표현하는것이 좋다.
let emptyArray5 = [1,2,3]
print(emptyArray5)

---결과---
[]
[]
[]
["Hello"]
["K", "S", "J"]
["KK", "Ss", "JJ"] 길이:[3]
["KK", "Ss", "JJ", "Hello!!"] 길이:[4]
["KK", "a", "b", "c"] 길이:[4]
[1, 2, 3]

~~~

__Dictionary를 확인해보자__{: style="color: #e26716"} - Key:Value형식의 데이터를 만들때 사용합니다.
데이터를 추가할경우 순서대로 들어가지 않습니다. 알아두세요.

~~~java

var str = "Hello, dictionary"
print(str)

var namsOf:[Int:String] = [1:"first",2:"second"]
print(namsOf)

var emptyDitionary = Dictionary<String, Int>()
var emptyDitionary2 = [String: Int]()

if emptyDitionary.isEmpty{
    print("not data")
}
if emptyDitionary2.isEmpty{
    print("not data")
}

emptyDitionary2["여자"] = 2
emptyDitionary2["남자"] = 5
print(emptyDitionary2)
print("==============")
emptyDitionary2 = ["동물":2]
print(emptyDitionary2)

---결과---
Hello, dictionary
[2: "second", 1: "first"]
not data
not data
["남자": 5, "여자": 2]
==============
["동물": 2]

~~~

__Dictionary를 확인해보자__{: style="color: #e26716"}

~~~java

//Basic Operators - 기본 연산자중 특이하게 보이는것만 정리했습니다. 


//튜플(Tuple)
let (x, y) = (1, 2)
print(x, y)

let httpStatus = (code: 404, description: "Not Found")
print(httpStatus)

print("======================")

//(a...b) a부터 b까지 반복하는 문법이다.
for index in 1...5 {
    print("{a...b}  \(index) times 5 is \(index * 5)")
}

print("======================")

//(a..<b) - a부터 b까지 가는데 b는 포함되지 않는다.
let names = ["Anna", "Alex", "Brian", "Jack"]
let count = names.count
for i in 0..<count {
    print("{0..<count}:[\(count)] i:[\(i)] is called \(names[i])")
}

---결과---
1 2
(code: 404, description: "Not Found")
======================
{a...b}  1 times 5 is 5
{a...b}  2 times 5 is 10
{a...b}  3 times 5 is 15
{a...b}  4 times 5 is 20
{a...b}  5 times 5 is 25
======================
{0..<count}:[4] i:[0] is called Anna
{0..<count}:[4] i:[1] is called Alex
{0..<count}:[4] i:[2] is called Brian
{0..<count}:[4] i:[3] is called Jack

~~~

Control Flow
<hr />

__for__{: style="color: #e26716"}

~~~java

//for
let names = ["Anna", "Alex", "Brian", "Jack"]
for name in names {
    print("Hello, \(name)!")
}
print("====================")
//for .. Dictionary
let numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
for (animalName, legCount) in numberOfLegs {
    print("\(animalName)s have \(legCount) legs")
}
print("====================")
//for closed ranges operator
for index in 1...5 {
    print("\(index) times 5 is \(index * 5)")
}


print("====================")
//for .. count를 10번 반복할때 아래와 같이 '_'를 써서 특별히 참조하는 숫자없이 count를 진행한다.
let base = 3
let power = 10
var answer = 1
for _ in 1...power {
    answer *= base
}
print("\(base) to the power of \(power) is \(answer)")

print("====================")
//for .. 0 ~ 59 까지 반복한다. half-open range operator
let minutes = 60
for tickMark in 0..<minutes {
    // render the tick mark each minute (60 times)
}
print("====================")
//for ..
let minuteInterval = 5
for tickMark in stride(from: 0, to: minutes, by: minuteInterval) {
    // stride(from:to:by:)- 0부터 to:60까지(half-open-range처럼 60은 해당되지 않는다.) 
    // by:5씩 늘어난다.
    // render the tick mark every 5 minutes (0, 5, 10, 15 ... 45, 50, 55)
}
print("====================")
let hours = 12
let hourInterval = 3
for tickMark in stride(from: 3, through: hours, by: hourInterval) {
    // stride(from:through:by:) - from:3부터 through:12(Closed-Range처럼 12까지 진행된다.) 
    // by:3씩 늘어난다.
    // render the tick mark every 3 hours (3, 6, 9, 12)
}
print("====================")

---결과---
Hello, Control Flow
Hello, Anna!
Hello, Alex!
Hello, Brian!
Hello, Jack!
====================
ants have 6 legs
spiders have 8 legs
cats have 4 legs
====================
1 times 5 is 5
2 times 5 is 10
3 times 5 is 15
4 times 5 is 20
5 times 5 is 25
====================
3 to the power of 10 is 59049
====================
위 주석참조
====================
위 주석참조
====================
위 주석참조
====================
~~~


__while__{: style="color: #e26716"} 

~~~java

var age = 0
while age < 5 {
    age += 1
    print(age)
}
---결과---
1
2
3
4
5

~~~

_가감, 증감 연산자는 swift2 이후로 사라졌습니다._{: style="color: red"}


__swich__{: style="color: #e26716"}

~~~java

// switch문에서 한개의 result를 공유할때 유의할점을 표현합니다.
let anotherCharacter: Character = "a"
switch anotherCharacter {
    case "a": // Invalid, the case has an empty body
    case "A":
        print("The letter A")
    default:
        print("Not the letter A")
}

// 아래 처럼 해야한다. 
let anotherCharacter: Character = "a"
switch anotherCharacter {
    case "a", "A":
        print("The letter A")
    default:
        print("Not the letter A")
}

print("====================")


//IOS Switch문에서는 Half-Open-Range도 사용가능합니다.
case 1..<5:
    naturalCount = "a few"

print("====================")


//Tuple을 이용한 Switch 
let somePoint = (1, 1)
switch somePoint {
    case (0, 0):
        print("\(somePoint) is at the origin")
    case (_, 0): // 앞쪽 '_' 어떤값이 와도 된다는 말이라고합니다.
        print("\(somePoint) is on the x-axis")
    case (0, _): // 뒷쪽 '_' 어떤값이 와도 된다는 말이라고 합니다.
        print("\(somePoint) is on the y-axis")
    case (-2...2, -2...2): // -2~2 사이 값이 온다면 된다.
        print("\(somePoint) is inside the box")
    default:
        print("\(somePoint) is outside of the box")
}
--결과--
// Prints "(1, 1) is inside the box

//Value Bindings in Switch
let anotherPoint = (2, 0)
switch anotherPoint {
    case (let x, 0): //let x에 2가 할당됩니다. == case (2, 0)
        print("on the x-axis with an x value of \(x)")
    case (0, let y): //let y에 0이 할당됩니다. == case (0, 0)
        print("on the y-axis with a y value of \(y)")
}
--결과--
// Prints "on the x-axis with an x value of 2


print("====================")


//Where in Switch - 추가적인 조건을 넣을때 사용합니다. 

let yetAnotherPoint = (1, -1)
switch yetAnotherPoint {
    case let (x, y) where x == y:
        print("(\(x), \(y)) is on the line x == y")
    case let (x, y) where x == -y:
        print("(\(x), \(y)) is on the line x == -y")
    case let (x, y):
        print("(\(x), \(y)) is just some arbitrary point")
}
--결과--
// Prints "(1, -1) is on the line x == -y


print("====================")


// Compound Cases
let someCharacter: Character = "e"
switch someCharacter {
    case "a", "e", "i", "o", "u":
        print("\(someCharacter) is a vowel")
    case "b", "c", "d", "f", "g", "h", "j", "k", "l", "m",
        "n", "p", "q", "r", "s", "t", "v", "w", "x", "y", "z":
        print("\(someCharacter) is a consonant")
    default:
        print("\(someCharacter) is not a vowel or a consonant")
}
--결과--
// Prints "e is a vowel

~~~


__Functions__{: style="color: #e26716"} 

~~~java

//func, Name:greet, Param:String, return:String
func greet(person: String) -> String{
    return "result"
}
print(greet(person: "person"))

//‘Function Argument Labels and Parameter Names’
func someFunction(firstParameterName: Int, secondParameterName: Int) {
    // In the function body, firstParameterName and secondParameterName
    // refer to the argument values for the first and second parameters.
}
someFunction(firstParameterName: 1, secondParameterName: 2)


// Argument Labels
func greet(person: String, from hometown: String) -> String {
    return "Hello \(person)!  Glad you could visit from \(hometown)."
}
print(greet(person: "Bill", from: "Cupertino"))
// Prints "Hello Bill!  Glad you could visit from Cupertino.


//Omitting Argument Labels
func someFunction(_ firstParameterName: Int, secondParameterName: Int) {
    // In the function body, firstParameterName and secondParameterName
    // refer to the argument values for the first and second parameters.
}
someFunction(1, secondParameterName: 2)

//_ Agrument Lablels를 사용 한것입니다.
func arithmeticMean(_ numbers: Double...) -> Double {
    var total: Double = 0
    for number in numbers {
        total += number
    }
    return total / Double(numbers.count)
}

//위에 Agrument Labels를 사용했기 때문에, Parameter를 적지 않고 바로 값을 나열해도 됩니다.
print(arithmeticMean(1, 2, 3, 4, 5))

-- Coding Test --
func sayHello(){
    print("func  Hello")
}
sayHello()


print("====================")


func sayHello2(name:String){
    print("func2 안녕 \(name)")
}
sayHello2(name:"sj")


print("====================")


func sayHello3(name:String) -> Bool{
    if name.isEmpty{
        return true
    }else{
        return false
    }
}
print("func3 \(sayHello3(name: "Sj"))")


print("====================")


func sayHello4(lastName name:String,_ age:Int = 18) -> String{
    return ("func4 안녕 \(name) 나는 \(age)살이야")
}
print(sayHello4(lastName: "Sj"))
print(sayHello4(lastName: "Sj",20))


print("====================")


func sayHello5(lastName name:String,nowAge age:Int = 18) -> String{
    return ("func5 안녕 \(name) 나는 \(age)살이야")
}
print(sayHello5(lastName: "Sj",nowAge: 33))


--결과--
func  Hello
====================
func2 안녕 sj
====================
func3 false
====================
func4 안녕 Sj 나는 18살이야
====================
func4 안녕 Sj 나는 20살이야
====================
func5 안녕 Sj 나는 33살이야
~~~

__Class__{: style="color: #e26716"}

~~~java

class Vehicle{
    //property 초기화를 꼭해야한다 (안하면 error발생된다)
    //stored property
    var currentSpeed = 0.0
    var description:String{
        return "Vehicle Description!!!"
    }
    
    func makeNoise(){
        print("소음발생")
    }
}

let someVehicle = Vehicle()
someVehicle.currentSpeed = 0.1
print(someVehicle.currentSpeed)
someVehicle.makeNoise()


print("========================")


class Bicycle:Vehicle{
    var hasBaskey = false
    override var description: String{
        return "자전거의 속도 \(currentSpeed)"
    }
   
    override func makeNoise() {
        print("자전거 소음 적음")
    }
}

let bicycle = Bicycle()
print(bicycle.currentSpeed)
bicycle.currentSpeed = 22.0
print(someVehicle.currentSpeed)
print(bicycle.currentSpeed)
print(bicycle.description)


print("========================")
class Train:Vehicle{
    override func makeNoise() {
        print("열차 소음 강함")
    }
}
let train = Train()
train.makeNoise()
print("========================")


class Car:Vehicle{
    var gear = 1
    override init() {
        print("차가 준비됐습니다.")
    }
    init(newGear:Int){
        gear = newGear
    }
}
let someCar = Car()

let someCar2 = Car(newGear: 5)
print("현재 차의 기아위치 : \(someCar2.gear)")
print("========================")


---결과---
0.1
소음발생
========================
0.0
0.1
22.0
자전거의 속도 22.0
========================
열차 소음 강함
========================
차가 준비됐습니다.
현재 차의 기아위치 : 5
========================

~~~


__Structure__{: style="color: #e26716"}

~~~java

print("========================")
var name = ["Kim", "Park", "Lee", "Kang"]
var age  = [20,19,21,22]
print("Array : 학생 정보 \(name[0]) \(age[0])")
print("========================")
struct Student {
    var name:String
    var age:Int
    
    var showInfomation: String{
       return "Student : 학생 정보 \(name) \(age)"
    }
}

var student = Student(name:"Kang", age:22)
print(student.showInfomation)
print("========================")
---결과---
========================
Array : 학생 정보 Kim 20
========================
Student : 학생 정보 Kang 22
========================

~~~


__Optionals__{: style="color: #e26716"}

~~~java

// 변수명 뒤에 '!' > Optional을 풀어주는것이다.
// '!' 변수가 nil일때는 사용하면 안되기때문에 유의해야한다.
//
let fNumber = "123"
let sNumber = Int(fNumber)

print("fNumber  > \(fNumber)")
print("sNumber  > \(sNumber)")

let ffString = "fff"
let ssNumber = Int(ffString)
print("ffString > \(ffString)")
print("ssNumber:Int(fff) > \(ssNumber)") // return nil : 형변환실패시 nil을 반환한다.

var serverCode: Int? = 404
serverCode = nil
// <<< 현재 nil상태이므로 '!' 사용시 error가 발생한다. 
// [Fatal error: Unexpectedly found nil while unwrapping an Optional value]
print("serverCode! > \(serverCode!)") 


---결과--- 
fNumber  > 123
sNumber  > Optional(123)
ffString > fff
ssNumber:Int(fff) > nil
Fatal error: Unexpectedly found nil while unwrapping an Optional value

~~~ -->