# JadenCase 문자열

## 내 풀이
 
~~~javascript
function solution(s) { 
    const arr = s.split(' ')
    function checkNum(char) {
        return (isNaN(char) ? char.toUpperCase() : char)
    }
    const upperCase = arr.map(ele => checkNum(ele[0]));
    for(i=0; i<arr.length; i++) {
        let disk = arr[i].split('');
        if(isNaN(upperCase[i])) {
          disk[0] = upperCase[i]  
        } else {
            console.log("one")
            return arr.join(' ')
        }
        arr[i] = disk.join('');
    }
    return arr.join(' ');

}
~~~

너무 복잡하게 풀었습니다. 이렇게까지 복잡할것이 아닌데..

## 베스트풀이

~~~javascript
function solution(s) {
  return s
    .toLowerCase()
    .split(" ")
    .map((v) => v.charAt(0).toUpperCase() + v.substring(1))
    .join(" ");
}
~~~

substring을 생각하지 못했습니다.. 대박..
너무 좋은 풀이입니다.