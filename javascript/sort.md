##js 中sort函数的用法

	//简单用法
	var arr1 =[1,9,5,7];
        arr1.sort();
        console.log(arr1);
			
	//输出：(4) [1, 5, 7, 9]





  	//升序
        var  arr = [{id:2},{id:1}];
        arr.sort(function(a,b){return a.id-b.id;});
        console.log(arr);
		
	//输出：(2) [Object,Object]-->[{id:1}[id:2}]
		

	//降序
        var  arr = [{id:2},{id:1},{id:3}];
        arr.sort(function(a,b){return a.id-b.id;});
        console.log(arr);
		
	//输出：(3) [Object,Object,Object]-->[{id:3}，{id:2}，{id:1}]
		
