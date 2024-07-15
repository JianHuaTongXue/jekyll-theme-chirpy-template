---
layout: post
title: TypeScript学习笔记
categories: [TypeScript, 游戏]
tags: [TypeScript, 学习笔记, 游戏]
date: 2016-10-13 11:11:24.000000000 +09:00
---
### 一、基础类型
 布尔值：

	let isDone:boolean = false;
	
 十进制：

	let decLiteral:number = 6;
	
 十六进制：

	let hexLiteral:number = 0xf00d;
	
 二进制：

	let binaryLiteral:number = 0b1010;
	
 八进制：

	let octalLiteral:number = 0o744;
	
 字符串：

	let name:string = "bob";
	name = "smith";
	
### 二、模板字符串：${ expr }

	
	let name:string = "Gene";
	let age:number = 37;
	let sentence:string = "Hello,my name is ${ name }.I'll be ${ age + 1 } year old next month.";
	let sentence:string = "Hello,my name is " + name + ".\n\n" + "I'll be " + (age + 1) + "years old next month.";
 
### 三、数组：

  方式一：

	let list:number[] = [1,2,3];

  方式二：

	let list:Array<number> = [1,2,3];

### 四、元组 Tuple：

 定义元组：

	let x:[string, number];
 正确赋值：

	x = ["hello", 10];
 错误赋值：

	x = [10 , "hello"];

### 五、枚举：(默认从0开始为元素编号)

	enum Color {Red, Green, Blue};
	let c: Color = Color.Green;
	
 改成从1开始编号:

	enum Color {Red = 1, Green, Blue};
	let c: Color = Color.Green;

 任意值：

	let noSure:any = 4;
	notSure = "maybe a string insted";
	notSure = false;

 数组：

	let list:any[] = [1,true,"free"];
	list[1] = 100;
	
 空值：

	function warnUser():void{
		alert("This is my warning message");
	}

### 六、const 声明：(被赋值后不能被改变)
	const numLivesForCat = 9;
	
### 七、解构数组：

	let input = [1,2];
	let [first,second] = input;
	console.log(first);
	console.log(second);
	
 在函数参数中使用：
	
	functionf([first,second]:[number,number]){
		console.log(first);
		console.log(second);
	}
	
	f(input);
	
 对象解构：

	let o = {
		a:"foo",
		b:12,
		c:"bar"
	}
	let [a,b] = o;

### 八、剩余变量...name的用法

	let [first,...rest] = [1,2,3,4];
	console.log(first); //输出 1
	console.log(rest); //输出 [ 2, 3, 4 ]

### 九、接口：

	interface LabelledValue{
		label:string;
	}
	
	function printLabel(labelledObj: LabelledValue){
		console.log(labelledObj.label);
	}
	
	let myObj = {size:10,label:"Size 10 Object"};
	printLabel(myObj);

 可选属性：（加？号）

	interface SquareConfig{
		color?:string;
		width?:number;
	}
	
	function createSquare(config:SquareConfig):{color: string; area:number}{
		let newSquare = {color: "White",area:100};
		if(config.color){
			newSquare.color = config.color;
		}
		
		if(config.width){
			newSquare.area = config.width * config.width;
		}
		return newSquare;
	}
	
	let mySquare = createSquare({color: "black"});

 只读属性：

	interface Point{
		readonly x:number;
		readonly y:number;
	}
	
	let p1:Point = {x: 10,y: 20}; // 初始化后不能再赋值
	
### 十、类：

	class Greeter {
		greeting: string;
		constructor(message: string){
			this.greeting = message;
		}
		greet(){
			return "Hello," + this.greeting;
		}
	}
	
	let greeter = new Greeter("world");
	
### 十一、继承：

	class Animal{
		name:string;
		constructor(theName:string){
			this.name = theName;
		}
		move(distanceInMeters:number = 0){
			console.log('${this.name} moved ${distanceInMeters}m.');
		}
	}
	
	class Snake extends Animal{
		constructor(name:string){
			super(name);
		}
		move(distanceInMeters = 5){
			console.log("Slithering...");
			super.move(distanceInMeters);
		}
	}
	
	class Horse extends Animl{
		constructor(name:string){
			super(name);
		}
		move(distanceInMeters = 45){
			console.log("Gallop...");
			super.move(distanceInMeters);
		}
	}
	
	let sam = new Snake("Sammy the Python");
	let tom:Animal = new Horse("Tommy the Palomino");
	
	sam.move();
	tom.move(34);
	
### 十二、访问修饰符：(默认为public)

 public公有的：
	
	class Animal{
		public name:string;
		public construtor(theName:string){
			this.name = theName;
		}
		public move(distanceInMeters: number){
			console.log('${this.name} moved ${distanceInMeters}m.');
		}
	}
	
 private私有的：
	
	class Animal{
		private name:string;
		constructor(theName:stirng){
			this.name = theName;
		}
	}
	
	new Animal("Cat").name;// 错误的使用，name属性是私有的
	
	protected受保护的：
	
	class Person{
		protected name:string;
		constructor(name:string){
			this.name = name;
		}
	}
	
	class Employee extends Person{
		private department:string;
		constructor(name:string,department:string){
			super(name);
			this.department = department;
		}
		
		public getElevatorPitch(){
			return 'Hello,my name is ${this.name} and I Work in ${this.department}.';
		}
	}
	
	let howard = new Employee("Howard","Sales");
	console.log(howard.getElevatorPitch);
	console.log(howard.name); //不能访问，只能在派生类中使用 可以在getElevatorPitch()中使用

### 十三、readonly 修饰符：

	class Octopus{
		readonly name:string;
		readonly numberOfLegs:number = 8;
		constructor(theName:string){
			this.name = theName;
		}
	}
	
	let dad = new Octopus("Man with the 8 strong legs");
	dad.name = "Man with the 3-piece suit";// 不能赋值，name属性是只读的，只能在声明是或构造函数里被初始化。
	
### 十四、getters/setters方法：

	let passcode = "secret passcode";
	
	class Employee{
		private _fullName:string;
		
		get fullName():string{
			return this._fullName;
		}
		
		set fullName(newName:string){
			if(passcode && passcode == "secret passcode"){
				this._fullName = newName;
			}
			else{
				console.log("Error: Unauthorized update of employee!");
			}
		}
	}
	
	let employee = new Employee();
	employee.fullName = "Bob Smith";
	if(employee.fullName){
		alert(employee.fullName);
	}
	
### 十五、静态属性static，使用类名调用

	class Grid{
		
		static origin = {x:0,y:0};
		
		calculateDistanceFromOrigin(point:{x:number;y:number;}){
			let xDist = (point.x - Grid.origin.x);
			let yDist = (point.x - Grid.origin.y);
			return Math.sqrt(xDist * yDist + yDist * yDist) / this.scale;
		}
		
		constructor(public scale:number){}
	}

### 十六、泛型：

	function identity<T>(arg:T):T{
		return arg;
	}
使用：

	let outpet =  identity<string>("myString");
	
	
