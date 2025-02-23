What is it?
-------------
시간표를 JavaScript를 사용하여 동적으로 생성할 수 있는 컴포넌트 입니다. 시간표의 그리드를 생성, 수업 오버레이의 표시등은 핵심 동작방식에 따라 생성되지만 사용자 요구에 따라 시작 시간, 종료 시간, 요일을 자유롭게 설정할 수 있습니다. 또한, 같은 수업명은 동일한 색상의 오버레이로 맵핑되는 편리함을 제공합니다.

여기서 테스트해보세요 : https://jeonwoohyuni.com/example/timetable.html

How to use
-----------

>아래의 방법을 사용하면 표준적인 컴포넌트의 사용법을 익힐수 있습니다. 더 나은 튜닝을 위해서는 코드의 동작방식을 이해하시기 바랍니다.

### 1. 시간표 생성하기
  시간표 요소를 추가할 DOM에 아래와 같은 HTML 코드를 추가하십시오.
  ```html
  <div class="timetable-mt">
        <div class="timetable-container">
            <div id="timetable" class="timetable">
                <div class="header"></div>
                <div class="header day-header">월</div>
                <div class="header day-header">화</div>
                <div class="header day-header">수</div>
                <div class="header day-header">목</div>
                <div class="header day-header">금</div>
                <!-- <div class="header day-header">토</div>
                     <div class="header day-header">일</div> -->
            </div>
        </div>
  </div>
  <script src="./load_timetable.js"></script> <!--js 직접 수정 필요-->
  ```
  기본적인 세팅은 월~금(평일)의 구조입니다. 주말이 필요한 경우 아래 토, 일 부분의 헤더 주석을 제거하세요

  필요한 문서 : [index.css]
  >*index.css의 내용은 필수 요소만 포함합니다. 필요시 전체를 복사하여 이동시키십시오.
  ```css
  .timetable {
    display: grid;
    grid-template-columns: auto repeat(5, 1fr); 
    /*요일 수에 따라 격자 그리드를 조정하세요*/
    gap: 1px;
    background-color: #ddd;
    border: 1px solid #ddd;
}
  ```
  요일의 수에 따라 격자 그리드의 생성 수를 조정해주어야합니다.
  ```css
  grid-template-columns: auto repeat(5, 1fr); /*기본 5일 5열 세팅*/
  ```

  ### 2. 요일과 시간대 설정하기
  필요한 문서 : [index.js]
  >*index.js의 내용은 필수 요소만 포함합니다. 필요시 전체를 복사하여 이동시키십시오.
  

  ```javascript
  const days = ['월', '화', '수', '목', '금'];
  const startHour = 9;
  const endHour = 19;
  ```
  - days 리스트 : 자신이 생성하고자 하는 그리드의 열 수 만큼 한글요일을 문자열로 작성해주세요.
  - startHour, endHour : 시작 시간과 종료 시간을 작성해주세요.

  >**<주의> days 리스트가 제대로 작성되지 않으면 시간표 정보를 정리하는 과정이 작동하지 않습니다**

  ### 3. 시간표 데이터 삽입하기
  courses 리스트에 데이터를 저장해야합니다.
  ```javascript
  const courses = [
  ];
  ```
  다음의 json 형식에 따라 데이터를 저장하십시오.
  ```
    {
        course_name : '테스트',
        schedule : '화 10:00~12:00',
        classroom : '공학관 0000'
    }
  ```

schedule 정보를 입력할때는 지정된 형식에 따라 입력해주어야합니다.

```
┌────────────┬────────────────────────────┐
  형식          예제                        
├────────────┼────────────────────────────┤
 올바른 형식    '화 10:00~12:00 목 10:00~12:00' 
             '월 09:00~10:30 수 13:00~15:00' 
├────────────┼────────────────────────────┤
 잘못된 예      '화10:00~12:00목10:00~12:00'   
              '화 10:00~12:00,목 10:00~12:00' 
              '화-10:00~12:00 목-10:00~12:00' 
├────────────┼────────────────────────────┤
 구분 규칙     ① (요일) (띄어쓰기) (시작시간)~(종료시간) 
             ② 여러 일정은 (띄어쓰기)로 구분                 
             예: '월 09:00~10:00 수 11:00~12:30' 
             ③ 요일과 시간 사이에 반드시 띄어쓰기 포함 
└────────────┴────────────────────────────┘

```
  
  ### 4. 동적으로 로드하기
  ```javascript
  load_timetable()
  ```
  다음 함수 선언에 의해 시간표가 동적으로 로드됩니다.
  ```javascript
  load_cell()
  ```
  courses에 시간표 정보가 추가되면 load_cell()을 선언하여 오버레이를 추가할 수 있습니다.

---
문의 : jjkw0511@mju.ac.kr
```
       _               __        __          _   _                   ___ 
      | | ___  ___  _ _\ \      / /__   ___ | | | |_   _ _   _ _ __ |_ _|
   _  | |/ _ \/ _ \| '_ \ \ /\ / / _ \ / _ \| |_| | | | | | | | '_ \ | | 
  | |_| |  __/ (_) | | | \ V  V / (_) | (_) |  _  | |_| | |_| | | | || | 
   \___/ \___|\___/|_| |_|\_/\_/ \___/ \___/|_| |_|\__, |\__,_|_| |_|___|
                                                   |___/ 
```