# 디자인패턴

## 24.02.13

```java
import java.util.*;

class Solution {
    public int[] solution(String[] id_list, String[] report, int k) {
        
        Map<String, Integer> map1 = new HashMap<>();
        
        for(int i = 0; i < id_list.length; i++){
            map1.put(id_list[i], i); //이름의 index 입력
        }
        
        Map<String, Set> map2 = new HashMap<>();
        
        for(String str : report){
            String reporter = str.split(" ")[0]; //신고자 명
            String suspect = str.split(" ")[1]; //용의자 명

            Set<String> set = null; //한 용의자에 대해 신고자 중복 제거
                
            if(map2.get(suspect) == null){
                set = new HashSet<>();
            } else {
                set = map2.get(suspect);
            }
            
            set.add(reporter);
            map2.put(suspect, set); //{용의자=신고자 set}
        }
        
        //System.out.println(map2.toString()); //해당 출력문을 넣게 되면 용량 초과이므로 채점할 때는 주석처리!
        
        int[] answer = new int[id_list.length];
        
        for(String key : map2.keySet()){
            if(map2.get(key).size() >= k) { //신고자의 수가 k보다 넘어갈때
                Iterator iter = map2.get(key).iterator();	// Iterator 사용
                while(iter.hasNext()) {
                    answer[map1.get(iter.next())] ++; //신고자의 번호를 찾아 1 더하기
                } 
            }    
        }     
        return answer;
    }
}
```
