## Scala 언어 공부시 참고
- 1. Scala 5분만에 배우기 :http://learnxinyminutes.com/docs/scala/
- 2. Coursera의 Scala 강의 : https://www.coursera.org/course/progfun
- 3. Scala School(트위터, 추천) : http://twitter.github.io/scala_school/ko/
- 4. 책 : programming in Scala(한국어판) : Scala의 창시자가 저술

## Spark 예제 docs참고해서 공부하기 
- http://homepage.cs.latrobe.edu.au/zhe/ZhenHeSparkRDDAPIExamples.html
- https://spark.apache.org/docs/latest/rdd-programming-guide.html

## Zeppelin 화재 데이터 예제
- https://github.com/uosdmlab/playdata-zeppelin-notebook

# 1. 대용량 처리기술의 개요 
## Intro
- 기술 : **컴퓨터 1대로 처리하지 못하므로 여러개로 분할하여 데이터를 저장하고 처리**하자
- 주로 구글 등 검색엔진 회사들이 웹 전체를 저장하고 처리하려다보니 기술개발이 필요하게 된다. 
- 구글이 이끌고 야후 등이 오픈소스를 통해 하둡을 적극 지원, 접근하기 쉬워지고 널리 쓰이기 시작함
- 빅데이터 기술은 대부분 하둡이라고 생각하고 봐도 무방하다. 
- SQL기반의 데이터는 거의 행렬 형태의 정형화된 데이터였으나 일반문서(웹문서)와 같이 비정형화된 데이터도 초점이 맞춰진다. 
- 사실 데이터를 저장만 해서는 쓸모가 없다. 데이터를 읽어들이고 변환하고 핵심을 추출하는 것도 마찬가지로 컴퓨터 1대로 할 수 있는 것보다 빨라져야 한다.(병렬 데이터 처리) 
- 맵리듀스 : 분산 데이터 처리
- 현재는 스파크(Apache Spark)가 많이 쓰인다. 

## 빅데이터 기술의 시초 및 작동방법 설명 
- 빅데이터의 시초1 : **GFS** - master과 이에 연결되어 있는 여러개의 slade로 이루어진 구조
- 빅데이터의 시초2 : **MapReduce** - 여러대의 분산 저장소에 존재하는 데이터를 변환하거나 계산하기 위한 프레임워크이다.  여러대의 컴퓨터에 데이터가 쌓여있을 때, 데이터를 요청받았을 때 각 컴퓨터에서 이를 읽어서 처리하도록 하는 것이다. functional programming의 map(저장된 것을 다른 것으로 바꾸는 작업), reduce(여러개를 하나로 합쳐서 하나로 만드는 작업) 함수를 조합하여 효율적으로 분산 환경에서 다양한 계산을 한다.
- 빅데이터의 시초3 : hadoop – GFS와 MapReduce 논문을 보고 이를 오픈소스로 구현한 것. 야후에서 프로젝트를 하던 중에 한 부분을 만듦. 이후 오픈소스로 공개함. 
- 하둡에는 HDFS와 MapReduce가 있다. HDFS는 GFS의 클론이다. 
- hadoop hive : SQL분석쿼리를 실행하면 이를 MapReduce코드로 변환해주는 도구이다. MapReduce는 코드를 작성하기 아주 불편하므로 이 프로젝트가 큰 인기를 끈다. 

## Apache Spark
- **Apache Spark** : 비교적 최근에 등장하여 선풍적인 인기를 끄는 분산처리 프레임워크. 메모리 기반의 처리를 통한 고성능과 functional programming(사용이 가능한 언어인 Scala) 인터페이스를 활용한 편리한 인터페이스가 특징이다. 
- 기존에는 데이터를 읽어서 연산을 하고 다시 데이터를 쓰면서 처리를 한다. 이것이 단순한 연산의 경우 나쁘진 않지만 반복연산을 할수록 효율이 떨어진다. 다단계의 연산이나 반복연산의 경우 중간 결과를 메모리에 저장하면 매번 디스크에 쓰는 것보다 훨씬 빠르다/ 
- 만약 한 대의 컴퓨터가 오류가 나면 메모리가 날아가므로 데이터를 복구하기가 어려웠는데 Spark에서는 이 부분까지 메꿈.
- Hadoop(MapReduce)는 매번 중간 결과를 디스크에 저장하지만 Spark는 이를 메모리에서 처리하므로 효율이 좋다. PageRank나 머신러닝 알고리즘같이 iteration이 여러번 돌 경우 

- 특히 성능이 좋다. 

## Apache Spark의 핵심개념 
1. RDD : “탄력적으로 분산된 데이터셋”
	- 오류 자동복구 기능이 포함된 가상의 리스트(데이터를 실제로 담지는 않는다. )
	(예를 들어 1TB데이터를 1GB의 리스트에 담아야 하는 경우. 사실상 불가능한 작업	임. 하지만 RDD방식을 사용하면 가능하다. 1TB데이터를 쪼개서 센 다음 나중에 합	해서 word count를 하는 방식을 채택하여 사용하게 된다. 즉 단순히 계산방법만을 	담고 있는 가상의 리스트를 의미한다고 본다. )
	- 다양한 계산의 수행이 가능, 메모리를 활용하여 높은 성능을 가진다. 
2. Scala Interface
	- 매우 간결한 표현이 가능한 모던 프로그래밍 언어
	- functional programming이 가능해 데이터의 변환을 효과적으로 표현이 가능하다
	- JAVA 프로그래밍과 호환이 가능하다. hadoop이 JAVA로 짜여 이것이 필요.

- 또한 Spark을 엔진으로 하는 확장 프로젝트들이 같이 제공된다.
	- spark SQL : Hive와 비슷하게 SQL로 데이터 분석(hive보다 성능좋다)
	- Spark Streaming : 실시간 분석(서버분석, 로그데이터분석 등)
	- MLlib : 머신러닝 라이브러리(알고리즘구현)
	- GraphX : 페이지랭크같은 그래프 분석


## Spark Streaming
- batches of input data를 RDD의 연속적인 과정으로 짧은 단위로 RDD를 보여줘 마치 실시간처럼 보여주는 형태

## 데이터분석 워크 플로우(Spark ver.)
![image](https://user-images.githubusercontent.com/49298791/75739273-cf49c180-5d47-11ea-8070-8956106fa00e.png)

# 2. Apache Spark 이론
## Scala부터 배워보자
- Spark는 Scala, python, java API모두 제공한다. 
- 간단히 말하면 Spark는 python스러운 JAVA이다. 
- Scala = Scalabel Language : 간결한 표현과 강력한 기능을 통해 더 큰 프로그램을 만들기 위한 언어. Scala가 가진 여러 가지 특징들이 데이터 분석하기에 좋은 것들이 많음.
![image](https://user-images.githubusercontent.com/49298791/75739290-dbce1a00-5d47-11ea-8dc6-12e66e59e8d1.png)

- Scala 코드를 컴파일 하면 Java와 마찬가지로 .class파일이 나온다
- JVM에서 실행이 가능하고 거의 동일한 실행성능을 가진다. 
- Java Class를 import하여 사용이 가능하다. 
- Java file과 Scala file을 혼용하여 컴파일도 가능하다. 
- 정적타입언어임. 실제 데이터분석할때는 시간이 오래걸리는데, 이미 컴파일 오류를 잡고 넘어갈 수 있기 때문에 더 나을 수 있다(동적언어보다), type inference하여 넣어 깔끔한 코드의 유지도 가능하다. 
- 간결한 문법, 강력한 표현력을 제공한다. (4)
1. 문법이 간결하면 좋다. 
- val file = spark.textfile(“hdfs://...”)
- val counts = file.flatMat(line => line.split(“ ”))
- 	.map(word=>(word,1))
- 	.reduceByKey(_+_)
- counts.saveAsTextFile(“hdfs://...”)

2. if-else분기 혹은 try-catch등이 모두 expression임.
- println(if (a==“A”) “It’s A” else “It’s not A”)
- val value = try{
- 	doSomeDangerousOperation
- 	} catch {
- 	case _ => “some value”}
3. 일관성있는 operator들
- //Java
- “A”.equals(“B”)
- //Scala
- “A”==“B”

4. 합리적인 class equality
- case class Person(name: String, work : String)
- val kevin=Person(“Kevin”, “Between”)
- val anotherKevein=Person(“Kevin”, ``Between``)
- kevin=anotherKevin //true

- Functional Programming의 의미 :기존의 프로그램의 함수가 아닌 수학에서의 함수를 생각한다. x인자를 넣으면 그에 맞는 y가 나온다. 함수를 데이터처럼 생각하여 파라메터로 넘기거나 조합하는 작업이 가능하다. y는 x가 정해지면 바꿀 수가 없다. ‘변수’가 없게 immutable하게 만들어야 한다. 이렇게 하면 버그가 줄어들고 한번 만든 함수를 믿을 수가 있다, 즉 immutable변수는 문제를 많이 단순화해준다(data share이나 paralielism에 강하다) 

## Scala 실습 코드 확인(map, reduce확인 : 함수형 처리)
- val a=1 //immutable value 
- var b=1 // variable
- //list(1,2,3) => *2 => list(2,4,6)
- list(1,2,3).map(x=>x*2) //다른형태의 리스트로 바꿔주는 map
- //list(1,2,3) => a+b => 6
- List(1,2,3).reduce((a,b)=> a+b)
- List(1,2,3,4,5).reduce((a,b)=>a+b)
- List(1,2,3). //전체 함수 리스트를 확인하는 방법
- List(1,2,3,4,5).filter(x=>x>3)

## 다시 Scala의 특성
- REPL(Real-Eval-Print-Loop).aka shell
- 새로운 언어를 빠르게 배우고 시험할 수 있다.
- 데이터를 들여다볼 경우 step-by-step으로 작업이 가능하다. 

## Scala의 특성
- 데이터의 구조
![image](https://user-images.githubusercontent.com/49298791/75739335-f7d1bb80-5d47-11ea-9cbb-29d2572c6ebd.png)

- tuple은 python에서의 개념과 동일
- Option은 null을 쓰지 않기위한 것. 값이 있을 때는 Some, 값이 없을 때는 None을 사용한다. 
- collection 다루기
![image](https://user-images.githubusercontent.com/49298791/75739353-028c5080-5d48-11ea-85d8-60cb6c85a55d.png)

 3. Spark 실습 1
- 설치 : java, java jdk, scala, spark설치 필요
- $> java –version
- $> javac –version
- $>cd C:\Program Files (x86)
- $>cd scala
- $> cd bin
- $> scala.bat // 스칼라 커널 완료
- 마찬가지로 hadoop, spark도 설치. tgz파일 압축는 widow에서 7zip으로 풀기
- ZhenHe예제를 통해 (구글링) : http://homepage.cs.latrobe.edu.au/zhe/ZhenHeSparkRDDAPIExamples.html
- map, filter, reduce, foreach, distinct, count, take를 예제로 공부하기
- 실행했을 때의 spark kernel의 형태
![image](https://user-images.githubusercontent.com/49298791/75739368-0c15b880-5d48-11ea-9872-42ca9be62d9b.png)

- map : Applies a transformation function on each item of the RDD and returns the result as a new RDD.
- scala> val a = sc.parallelize(List("dog", "salmon", "salmon", "rat", "elephant"), 3)
- a: org.apache.spark.rdd.RDD[String] = ParallelCollectionRDD[0] at parallelize at <console>:24

- scala> val b = a.map(_.length)
- b: org.apache.spark.rdd.RDD[Int] = MapPartitionsRDD[1] at map at <console>:25

- scala> val c = a.zip(b)
- c: org.apache.spark.rdd.RDD[(String, Int)] = ZippedPartitionsRDD2[2] at zip at <console>:27

- scala> c.collect
- [Stage 0:>                                                          (0 + 3) /                                                                             res2 Array[(String, Int)] = Array((dog,3), (salmon,6), (salmon,6), (rat,3), (elephant,8))

- functional literal
![image](https://user-images.githubusercontent.com/49298791/75739400-1afc6b00-5d48-11ea-8a83-5eeb78773306.png)

![image](https://user-images.githubusercontent.com/49298791/75739416-22bc0f80-5d48-11ea-8b31-917b782bf33f.png)

# Spark 동작원리 & 실습
## Spark의 핵심개념
- 1. RDD : 클러스터 전체에서 공유되는 리스트, 메모리상에 올라간다(메모리가 부족하면 디스크에 spill), map, reduce, count, filter, join 등의 작업이 모두 가능하다. 여러 작업을 설정하고 결과를 얻을 때는 lazy하게 연산이 된다(action계산시에 느리게 계산이 된다는 의미). 리스트에 데이터를 가지고 있는 것이 아님. 어떻게 map하는지의 방법만 알고 있다가 나중에 값을 받아서 action을 취하는 방식을 사용한다. 
- 2. Scala : 데이터 분석하기에 아주 좋은 언어, 
<br>
## RDD의 작동방식
- transformations : 데이터를 어떻게 구해낼질르 표현
- Actions : 표현된 데이터를 가져온다
- Linage : 클러스터 중 일부의 고장으로 작업이 중간에 실패하더라고 Lineage를 통해 데이터를 복구한다. 

- Lazy Execution : transformation시에는 계산을 수행하지 않고 Action이 수행되는 시점부터 데이터를 읽어들여 계산을 시작하는 형태이다. 

## RDD Transformation
- Rdd의 데이터를 다른 형태로 변환하는 것을 의미한다. : 실제로 데이터가 변환이 되는 것이 아니라 데이터를 읽어 어떻게 바꾸는지의 “방식”을 기록하는 것. 실제 변환은 Action이 수행되는 시점에서 이루어딘다. 
- map, filter, flatMap, mapPartitions, sample, union, intersection, distinct, groupByKey, reduceByKey, join, repartition 등
- 최신의 자료 참고: https://spark.apache.org/docs/latest/rdd-programmingguide.html

## RDD Action
- 여러 가지 변환(Transformation)이 담긴 RDD의 정보를 통한 계산을 수행한다
- reduce, collect, count, first, take, saveAsTextFile, countByKey, foreach 등
- 최신의 자료 참고 : https://spark.apache.org/docs/latest/rdd-programmingguide.html

## word count example : 해당 단어가 몇 번 나왔는지를 세어 목록을 만드는 예제
- scala> val rdd = sc.textFile(“C:/Users/user/Desktop/spark-3.0.0-preview2-bin-hadoop2.7/python/README.md”) //실제 파일 불러오는 과정 
- scala> rdd.first //action을 취하는 과정 
- scala> rdd.map( line => line.split(“ ”) )
- scala> res18.take(3)
- scala> rdd.flatMap( line => line.split(“ ”))
- scala> res21.take(10)
- //이제는 단어를 하나씩 세볼 예정
- scala> rdd.flatMap( line => line.split(“ ”) ).map(w => (w,1) ) //단어 하나하나
- scala> res11.take(10) //결과 확인
- scala> rdd.flatMap( line => line.split(“ ”) ).map(w => (w,1) ).reduceByKey( (a,b) => a+b ) //실제 단어들의 수를 count
- scala> res13.take(10)
- scala> rdd.flatMap( line => line.split(“ ”) ).map(w => (w,1) ).reduceByKey( (a,b) => a+b ).sortBy( t => t._2, false).take(10) //tuple에서 2번째 있는 값기준 정렬
-scala> rdd.flatMap( _split(“ ”) ).map(w => (w,1) ).reduceByKey( _+_ ).sortBy( t => t._2, false).take(10) //placeholder로 다시 코드 작성하는 경우 
- scala> rdd.flatMap(_.split(“ ”)).map( w=> (w,1)).countByKey
## Data Loading
- Spark의 데이터 입력 부분은 Hadoop코드를 그대로 사용하기 때문에, Hadoop에서 지원하는 모든 소스를 사용이 가능하다. 
- 로컬 파일 : sc.textFile(“file:///...”)
- HDFS : sc.textFile(“hdfs://...”)
- Amazon S3 : sc.textFile(“s3://...”)
- HBase, Cassandra : Spark HBase Connector등 이용
- 또한 압축파일을 읽어들이는 기능, 와일드카드(*)의 사용도 가능하다. 


## Transformation : Map, Filter
- map이나 filter와 같은 transformation은 즉시 계산이 수행되는 것이 아니라, count()와 같은 Action이 수행될 때 실제 계산이 수행되는 방식이다.
- transformation이 기록된 새 RDD를 리턴해준다. 
- map(func) : func으로 기술되는 동작을 RDD에 모든 element에 수행.
- filter(func) : true/false를 판별하는 func이 true인 element만 남긴다. 
- 실제로 기록만 남기는 과정이기 때문에 아무리 데이터가 커도 빠른시간 내에 기록이 가능한 부분이다. 

## Transformation : Reduce, GroupBy
- reduce(func) :기술한 func대로 RDD의 element를 합치는 작업을 수행
- map()은 각 클러스터 간 데이터 교환이 없이 element-wise데이터 변환만을 수행하므로 아주 효율적인 병렬처리가 가능하다, 하나의 클러스터 내부의 동작만 정의하고 각 클러스터간 데이터의 교환이 일어날 필요가 전혀 없다. 
- reduce()도 최종적으로 클러스터간의 데이터가 모이기 전에 클러스터 내부의 데이터로부터 reduce계산이 가능하기에 효율적인 operation임. 즉 최종적인 데이터만 한번에 처리하면 되는 것이고 나머지의 각 과정은 각 컴퓨터에서 처리가 가능하게 할 수가 있다. 
- groupBy(func) : reduce와 비슷하나 데이터를 줄이는 것이 아니라 전부 보존해서 수집해야 한다. 대량의 네트워크 트래픽이 발생하고 메모리 문제가 발생할 수 있다. groupBy자체는 transformation, rdd.groupBy(_._1).collect.foreach(println)이 되어야 action이 처리가 된다. 

## Action : Count, Collect, Take 등
- Spark는 Action이 수행되면 그때서야 파일을 로드하고, 기록된 transformation을 수행하고 최종 action을 수행한다
- count() : RDD의 element를 세는 동작
- collect() : RDD의 내용을 전부 드라이버 프로그램으로 가져온다. RDD의 내용이 큰 경우에 collect를 하면 메모리가 꽉차서 프로그램이 죽을 수도 있다. RDD의 내용이 충분히 작지 않으면 안전한 take를 사용한다. 
- take(n) : 처음 n개의 element를 가져오는 작업

## RDD 캐싱
- Spark이 메모리를 사용하는 건데 메모리를 어떻게 사용하느냐
- spark는 메모리 캐시를 활용하여 성능을 극대화할 수 있다. 
- 페이지랭크같이 iteration 돌면서 실시간으로 구현하는 것은 극적으로 메모리를 사용하면 성능이 좋다. 하지만 그렇지 않은 경우에는 속도면에서 큰 차이는 없다고 한다. 
- 명시적으로 메모리에 올리는 작업을 RDD 캐싱이라고 한다. 
- persist() 나 cache()를 이용하여 메모리에 작업을 올린다. 
- 데이터가 너무 커서 메모리에 올라가지 않는 경우는 캐시하지 못한 데이터는 다시 계산한다. 
- 스파크에서는 RDD가 action으로 수행될 때마다 소스에서 다시 로드하여 사용한다. 이 방법이 아니라 메모리에 올려서 사용하는 방식.
- 기본 디폴트로 메모리에 저장하고 옵션으로 디스크를 지정할 수 있다. 
- docs : https://spark.apache.org/docs/latest/index.html : Which Storage Level to Choose?를 참고하기 

## Spark Cluster Mode
- hadoop의 전형적인 Master-Slave 구조이다. 
- spark를 local(한대의 사용자 컴퓨터) thread에서 실행할 수도 있지만 대부분은 여러대의 컴퓨터로 작성하는 cluster환경을 사용하는 것이 일반적이다. 
- spark App는 cluster에서 독립적인 프로세스의 집합으로 실행된다. 이들은 Driver Progrom의 Spark Context를 통해 조정된다. 
- cluster환경에서 실행하기 위해 Spark Context는 몇가지 타입의 cluster manager들에 저속할 수 있다. 
- 일단 연결되면 Spark는 Cluster의 노드에서 Excutor(사용자가 만든 어플리케이션에 대한 계산을 실행하고 데이터를 저장한느 프로세스)를 요구한다. 
- 다음으로 사용자가 작성한 어플리케이션 코드를 excutor에게 보낸다. 
- 마지막으로 sparkContext가 Excutor들에게 task를 보내 실행함 
- Cluster Manager
- Driver(Master) : sparkContext(App)이 구동됨
- Worker(Slave) : Excutor가 구동됨, 여러개의 task가 수행
- 다음은 Standalone Cluster 실행순서이다. 
0. bin/spark-submit을 이용하여 Application 제출
1. Driver Program이 실행되고 SparkContext(Spark Cluster와의 연결)가 생성됨
2. Driver가 Cluster Manager에게 Executor 리소스 요청
3. Cluster Manager가 Worker들에게 Executor를 띄우도록 명령함
4. Driver Program이 DAG Scheduling을 통해 작업을 Task 단위로 분할하여 할당된 Executor에게 전송
5. Executor가 여러 스레드에서 Task들을 실행한 후 결과를 Driver Program에 보냄
6. Application이 종료되면 Cluster Manager에게 리소스 반납

![image](https://user-images.githubusercontent.com/49298791/75739460-42533800-5d48-11ea-873a-ddb3ea54a57c.png)


## RDD Api(ZhenHe libaray참고하여 예제 계속 연습해야 한다. )
- RDD : 클러스터에서 구동시에 여러 데이터 파티션은 여러 노드에 위치할 수 있다. RDD를 사용하면 모든 파티션의 데이터에 접근하여 Computation 및 transformation을 수행할 수 있다. 
- RDD의 일부가 손실되면 lineage information을 사용해 손실된 파티션의 데이터를 재구성할 수 있다. 
- 1. DoubleRDDfunction : 숫자값 aggregation의 다양한 방법을 포함한다. 
- 2. PairRDDFunction : Tuple구조가 있을 때 사용한다 
- 3. OrderedRDDFunctions : Tuple구조이며 Key가 implicitly정렬가능할 경우 사용한다. 
- 4. SequenceFiledRddFunctions : 하둡의 Sequence 파일을 만들 수 있는 방법을 포함한다. PairRDDFunction처럼 Tupe구조가 필요하다. Tuple을 쓰기 가능한 유형으로 변환될 수 있도록 추가 요구사항이 존재한다. 
- 참고로 sc.parallelize()를 사용하는 것은 spark에서 input data의 새로운 집합을 생성하라는 것과 동일한데 예를 들어 sc.parallelize(data, 8)이면 데이터가 메모리에 저장될 때 데이터를 8조각으로 쪼개서 메모리에 저장하라는 뜻과 동일하다. 
- parallelize collections are created by calling sparkContext’s parallelize method on an existing collection in your driven program(a Scala Seq). 
- One important parameter for parallel collections is the number of partitins to cut the dataset into. 

- The behavior of the above code is undefined, and mat not work as intended, to excute jobs, Spark breaks up the processing of RDD operations into tasks, each of which is excuted by an executor. Prior to execution, Spark computes the task’s closure. the closure is those variables and methods which must be visible for the excutor to perform its computations on the RDD. This closure is serialized and sent to each executor.


# Apache Zeppelin 연동
- 자체적으로 서버를 가지고 있으면서 code입력시 visualize도 유용한 형태이다. 
```scala
//사용가능확인
sc.version

//데이터
sqlContext
val rdd = sc.makeRDD( 0 to 100 ).map( x => (x, 200-x))
rdd.take(20)

//정형화된 데이터를 다루기 위한 dataframe화 
val df = rdd.toDF("index","value")
df.registerTempTable("spark_study") //sparksql사용할 준비가 된것

//SparkSql코드짜는 방법
//예1.
sql.Context.sql("select * from spark_study")
//예2.
%sql
select * from spark_study //시각화 등도 한번에 같이 가능하다. 
//예3.
%sql
select *
from spark_study
where value > 120
```