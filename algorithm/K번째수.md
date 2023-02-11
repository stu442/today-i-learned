# K번째수

## 내 풀이

~~~javascript
function solution(array, commands) {
    // arr의 commands[i][0]-1 ~ commands[i][1] 번째를 자릅니다.(slice)
    // slice 된 배열을 정렬합니다.
    // 그중 commands[i][2]-1 번째 숫자를 새로운 배열에 담습니다.
    // 이걸 commands 의 갯수 만큼 반복합니다.
    
    let answer = []
    
    for(i=0; i<commands.length; i++){
        answer.push(array.slice(commands[i][0]-1, commands[i][1]).sort((a,b) => a - b)[commands[i][2]-1])
    }
    
    return answer
}
~~~

slice 가 굉장히 헷갈렸습니다.

arr.slice(begin, end)인데, slice 는 end 를 제외하고 추출합니다.

이게 굉장히 헷갈리는데 begin 은 포함해서 추출하기 때문입니다.

기억 잘해야합니다.

## 베스트 풀이

~~~javascript
function solution(array, commands) {
    return commands.map(command => {
        const [sPosition, ePosition, position] = command
        const newArray = array
            .filter((value, fIndex) => fIndex >= sPosition - 1 && fIndex <= ePosition - 1)
            .sort((a,b) => a - b)    

        return newArray[position - 1]
    })
}
~~~

구조분해할당을 쓰면 좋겠다 생각은 했지만, 실행은 하지 않았습니다.

아... 이렇게 쓰면 되는 거였군요.

map을 통해 command 에 요소를 하나씩 가져옵니다. 그러고 분해할당을 합니다.

filter 를 통해 요소들을 복사해주고,

sort로 정렬을 한 후 에 반환합니다.

천재...
