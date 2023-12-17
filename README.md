![bg right:40% 80%](https://cdn.discordapp.com/attachments/1061501682741157890/1185482580724830218/icons.png?ex=658fc5b0&is=657d50b0&hm=9eeb6016d340bde68bae5730d2ef9a40232d1d075a0c78acf88d076610a5db4e&)

# 기말 프로젝트

**스마트폰앱개발 04분반**

20203264 정승민

---
# 목차

- 프로젝트 소개 **(UP & DOWN 게임)**

- 프로젝트 개발 과정

- 레이아웃 구성 (xml)

- 소스코드 설명 (java)

- 애플리케이션 실행

- 소감 및 피드백


---
# 프로젝트 소개 (UP & DOWN 게임)
- 숫자 범위를 지정하고 그 사이에 있는 **지정된 숫자**를 맞추면 성공.

- 입력한 숫자가 지정된 숫자보다 낮으면 **Down**.

- 입력한 숫자가 지정된 숫자보다 높으면 **Up**.

- **지정된 숫자를 맞출 때까지** 게임을 계속 진행.

- 난이도는 총 **2가지**로 구성.
    - **1부터 50**까지의 숫자를 맞추는 **NORMAL** 난이도
    - **1부터 100**까지의 숫자를 맞추는 **HARD** 난이도



---
# 프로젝트 개발 과정

- 개발환경 : **Android Studio Dolphin, PhotoShop 2023**

- MainActivity : **타이틀 화면** 및 **난이도 선택** 구현 
    - 직접 풀어보기 10-1 변형 **(난이도 선택 기능 구현)**

- SecondActivity & ThirdActivity : **UP & DOWN 게임 구현**
    - **교재 4장, 5장** 학습 내용을 바탕으로 구현
    - **노멀 난이도** : **SecondActivity** / **하드 난이도** : **ThirdActivity**

- 포토샵을 이용하여 **앱 아이콘 디자인 작업** 진행    

---

# activity_main.xml

![bg right:40% 80%](https://cdn.discordapp.com/attachments/1061501682741157890/1185515794608705556/image.png?ex=658fe49f&is=657d6f9f&hm=57943d7e0c77d0404afa90a59191e04770d9340d0b8bd06c381ca444a84735f7&)

- 직접 풀어보기 10-1 변형

- 이미지뷰 + 라디오 버튼 2개 -> 라디오 그룹으로 구성

- 난이도를 선택하고 게임 시작 버튼을 누르면 게임 시작

---

# second/third.xml

![bg right:40% 80%](https://cdn.discordapp.com/attachments/1061501682741157890/1185517227991117954/image.png?ex=658fe5f4&is=657d70f4&hm=ff6d4df42901c4f92ad92cc9beeac89948af761d4fa76d6ee0e3f68de4467d76&)

- 교재 **4장과 5장** 참고

- **다중 리니어레이아웃** 활용

- **텍스트뷰**로 맞춰야 할 숫자 범위 표시

- **숫자 생성** / **결과 확인** 버튼 표시

- 입력 부분은 **에디트텍스트**로 구현

- 힌트는 **텍스트뷰**와 **이미지뷰**를 통해 확인 가능

---

# MainActivity.java

```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        setTitle("UP & DOWN 게임");
        ImageView imageView = (ImageView) findViewById(R.id.imageView);
        final RadioButton rdoSecond = (RadioButton) findViewById(R.id.rdoSecond);
        Button btnNewActivity = (Button) findViewById(R.id.btnNewActivity);

        btnNewActivity.setOnClickListener(new View.OnClickListener() {
            public void onClick(View v) {
                Intent intent;

                if (rdoSecond.isChecked() == true)
                    intent = new Intent(getApplicationContext(), SecondActivity.class); // NORMAL 난이도 선택

                else
                    intent = new Intent(getApplicationContext(), ThirdActivity.class); // HARD 난이도 선택

                startActivity(intent);
            }
        });
    }
}
```

# Second/ThirdActivity.java

```java
public class SecondActivity extends AppCompatActivity {

    EditText edtNum; // 숫자 입력
    Button btnStart, btnResult; // 숫자 생성, 결과 확인 버튼
    TextView tvHint; // 힌트 메시지 출력
    ImageView ivGame; // 힌트 이미지 출력

    // 필요한 변수 선언
    int count; // 카운트 횟수 저장
    int number, answer; // number = 입력한 숫자, answer = 지정된 숫자
    Random rand = new Random(); // 랜덤 함수 선언

 @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_second);
        setTitle("UP & DOWN 게임 (NORMAL)");

        edtNum = findViewById(R.id.edtNum);
        btnStart = findViewById(R.id.btnStart);
        btnResult = findViewById(R.id.btnResult);
        tvHint = findViewById(R.id.tvHint);
        ivGame = findViewById(R.id.ivGame);
        tvHint.setText("숫자 생성 버튼을 누르면\n게임이 시작됩니다.");

        // 게임 시작 버튼 클릭 시 작동
        btnStart.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                count = 0;  // 카운트 횟수 초기화
                answer = rand.nextInt(50)+1; // Third에서는 100으로 조정
                tvHint.setText("게임이 시작되었습니다.");
                btnResult.setEnabled(true); // 결과 확인 버튼 활성화
                btnStart.setEnabled(false); // 숫자 생성 버튼 비활성화
                ivGame.setImageResource(R.drawable.icons);
            }
        });

        // 결과 확인 버튼 클릭 시 작동
        btnResult.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                number = Integer.parseInt(edtNum.getText().toString()); // 숫자 입력

                // 일정 숫자 범위를 벗어나면 작동
                 if (number <= 0 || number > 50) {
                    tvHint.setText("1부터 50까지만 입력 가능합니다.");
                }

                 // 아무 숫자도 입력하지 않으면 작동 (작동 불가)
                else if (edtNum.getText().toString() == null) {
                    Toast.makeText(SecondActivity.this,"숫자를 입력하세요.",Toast.LENGTH_SHORT).show();
                }

                 // 시도 횟수를 카운트하는 코드
                else {
                    count++;

                    // 입력한 숫자가 지정된 숫자보다 낮을 때 작동
                    if (number > answer) {
                        tvHint.setText("DOWN!\n더 작은 수를 입력하세요.");
                        ivGame.setImageResource(R.drawable.down);
                    }

                    // 입력한 숫자가 지정된 숫자보다 높을 때 작동
                    else if (number < answer) {
                        tvHint.setText("UP!\n더 큰 수를 입력하세요.");
                        ivGame.setImageResource(R.drawable.up);
                    }

                    // 입력한 숫자와 지정된 숫자가 일치할 때 작동
                    else {
                        tvHint.setText("축하합니다. 정답을 맞췄습니다.\n시도 횟수는 " + count + "회입니다.");
                        ivGame.setImageResource(R.drawable.congratulation);
                        btnStart.setEnabled(true); // 숫자 생성 버튼 활성화
                        btnResult.setEnabled(false); // 결과 확인 버튼 비활성화
                    }

                    // 가장 마지막에 입력한 숫자 표시
                    edtNum.setHint(edtNum.getText().toString());
                    edtNum.setText("");
                }
            }
        });
    }
}
```
---

# 애플리케이션 실행

[![Video Label](https://cdn.discordapp.com/attachments/1061501682741157890/1185727802612404345/Screenshot_20231217_083545__.png?ex=6590aa11&is=657e3511&hm=bced87c306eb611efd347fd9bebdb16ce29f1ca5b413f507554a94d5680b57ac&)](https://youtu.be/l3N-3laiKc0)

---

# 소감 및 피드백

- 개발 기간은 예상보다 길었지만, 주어진 기간 안에 프로젝트 완성

- 이번 프로젝트를 통해 앱개발 역량 향상

- UP & DOWN 게임에 대한 알고리즘 숙지 완료

- 시간 여유 있다면 종강 후 오류 수정 등의 작업 진행 예정
