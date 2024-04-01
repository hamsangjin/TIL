# 문제 1 - 최대값 판별하기
사용자로부터 두 개의 숫자를 입력받아, 더 큰 숫자를 출력하는 프로그램을 작성하세요.
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <script>
        let number1 = prompt("첫 번째 숫자를 입력하세요: ");
        let number2 = prompt("두 번째 숫자를 입력하세요: ");

        console.log(Math.max(number1, number2));
    </script>
</body>
</html>
```

<br>

---

<br>


# 문제 2 - 성적 분류하기
사용자로부터 점수를 입력받아, 해당 점수가 90점 이상이면 'A', 80점 이상 90점 미만이면 'B', 70점 이상 80점 미만이면 'C', 60점 이상 70점 미만이면 'D', 그 외의 경우 'F'를 출력하는 프로그램을 작성하세요.
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <script>
        let score = parseInt(prompt("점수를 입력해주세요.")); // 입력값을 정수형으로 변환

        switch(Math.floor(score/10)){
            case 10:
            case 9:
                console.log('A');
                break;
            case 8:
                console.log('B');
                break;
            case 7:
                console.log('C');
                break;
            case 6:
                console.log('D');
                break;
            default:
                console.log('F');
        }
    </script>
</body>
</html>
```

<br>

---

<br>

# 문제 3 - 계절 판별하기
사용자로부터 월을 입력받아, 해당 월이 어떤 계절에 속하는지 출력하는 프로그램을 작성하세요. 예를 들어, 12, 1, 2월은 '겨울', 3, 4, 5월은 '봄', 6, 7, 8월은 '여름', 9, 10, 11월은 '가을'에 해당합니다.
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <script>
        let month= prompt("월을 입력해주세요.");

        switch(parseInt(month)){       
            case 12:
            case 1:
            case 2:
                console.log("겨울");
                break;
            case 3:
            case 4:
            case 5:
                console.log("봄");
                break;
            case 6:
            case 7:
            case 8:
                console.log("여름");
                break;
            case 9:
            case 10:
            case 11:
                console.log("가을");
                break;
        }
    </script>
</body>
</html>
```
