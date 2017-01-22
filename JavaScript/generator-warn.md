
```javascript
 function *foo(){
    a++;
    yield;
    b = b * a;
    a = (yield b) + 3;
  }

  function *bar(){
    b--;
    yield;
    a = (yield 8) + b;
    b = a * (yield 2);
    console.log(a);
  }

  let step = (gen) =>{
    var it = gen(),
        last;

    return () =>{
      last = it.next(last).value;
    }
  };

  var a = 1,
      b = 2,
      s1 = step(foo),
      s2 = step(bar);

  s2(); // a = 1, b = 1
  s2(); // a = 1, b = 1, last = 8
  s1(); // a = 2, b = 1
  s2(); // a = 9, b = 1, last = 2

  s1(); //a = 9, b = 9, last = 9
  s1(); // a = 12, b = 9
  s2(); //a = 12, b = 24

  console.log(a, b);
```


