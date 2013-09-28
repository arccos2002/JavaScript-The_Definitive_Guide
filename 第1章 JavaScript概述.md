# 第1章 JavaScript概述

## 1.1 JavaScript语言核心

    // 定义一个构造函数以初始化一个新的Point对象
    function Point(x,y){
        this.x = x;
        this.y = y;
    }
    
    // 使用new关键字和构造函数来创建一个实例
    var p = new Point(1,1)
    
    // 通过给构造函数的prototype对象赋值来给Point对象定义方法（实例方法）
    Point.prototype.r = function(){
        return Math.sqrt(this.x * this.x + this.y * this.y);
    };
    
    // Point的实例对象p（以及所有的Point实例对象）继承了方法r()
    p.r();
