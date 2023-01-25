-- 참고1: 통합쿼리함수 정보 사이트: https://cloud.google.com/bigquery/docs/reference/standard-sql/federated_query_functions?hl=ko

----------------------------------------------- 5강. 하나의 값 조작하기 -------------------------------------------------------

---- lec5_1. 코드 값을 레이블로 변경하기 ----

-- lec5_1테이블 만들기
DROP TABLE IF EXISTS inlaid-lane-373607.sqlr202301.lec5_1;
CREATE TABLE inlaid-lane-373607.sqlr202301.lec5_1(
    user_id         string(255)
  , register_date   string(255)
  , register_device integer
);

INSERT INTO inlaid-lane-373607.sqlr202301.lec5_1
VALUES
    ('U001', '2016-08-26', 1)
  , ('U002', '2016-08-26', 2)
  , ('U003', '2016-08-27', 3)
;

-- 1) 등록 코드(register_device)가 1이면 데스크톱, 2면 스마트폰, 3이면 애플리케이션이라는 레이블로 변경하는 작업 수행 / user_id와 레이블만 보이게끔 select문으로 설정
select user_id,
case when register_device = 1 then '데스크톱'
when register_device = 2 then '스마트폰'
when register_device = 3 then '애플리케이션'
else ''
end as device_name
from inlaid-lane-373607.sqlr202301.lec5_1;


---- lec 5_2. url에서 요소 추출하기 ----

-- lec5_2 테이블 만들기
DROP TABLE IF EXISTS inlaid-lane-373607.sqlr202301.lec5_2;
CREATE TABLE inlaid-lane-373607.sqlr202301.lec5_2(
    stamp    string(255)
  , referrer string
  , url      string
);

INSERT INTO inlaid-lane-373607.sqlr202301.lec5_2
VALUES
    ('2016-08-26 12:02:00', 'http://www.other.com/path1/index.php?k1=v1&k2=v2#Ref1', 'http://www.example.com/video/detail?id=001')
  , ('2016-08-26 12:02:01', 'http://www.other.net/path1/index.php?k1=v1&k2=v2#Ref1', 'http://www.example.com/video#ref'          )
  , ('2016-08-26 12:02:01', 'https://www.other.com/'                               , 'http://www.example.com/book/detail?id=002' )
;

-- 1) 어떤 웹 페이지를 거쳐 넘어왔는지 판별하기
select stamp, net.host(referrer) as referrer_host
from inlaid-lane-373607.sqlr202301.lec5_2;

-- 2) url에서 경로와 요청 매개변수 값 추출하기
select stamp, url, regexp_extract(url, '//[^/]+([^?#]+)') AS path, regexp_extract(url, 'id=([^&]*)') as id
from inlaid-lane-373607.sqlr202301.lec5_2;



---- lec 5_3. 문자열을 배열로 분해하기 ----

-- 테이블 만들기
DROP TABLE IF EXISTS inlaid-lane-373607.sqlr202301.lec5_3;
CREATE TABLE inlaid-lane-373607.sqlr202301.lec5_3(
    stamp    string(255)
  , referrer string
  , url      string
);

INSERT INTO inlaid-lane-373607.sqlr202301.lec5_3
VALUES
    ('2016-08-26 12:02:00', 'http://www.other.com/path1/index.php?k1=v1&k2=v2#Ref1', 'http://www.example.com/video/detail?id=001')
  , ('2016-08-26 12:02:01', 'http://www.other.net/path1/index.php?k1=v1&k2=v2#Ref1', 'http://www.example.com/video#ref'          )
  , ('2016-08-26 12:02:01', 'https://www.other.com/'                               , 'http://www.example.com/book/detail?id=002' )
;

-- 1) url 경로를 슬래시로 분할해서 계층을 추출하기
select stamp, url,
split(regexp_extract(url, '//[^/]+([^?#]+)'), '/')[SAFE_ORDINAL(2)] AS path1
, split(regexp_extract(url, '//[^/]+([^?#]+)'), '/')[SAFE_ORDINAL(3)] AS path2
from inlaid-lane-373607.sqlr202301.lec5_3;



---- 5_4. 날짜와 타임스탬프 다루기 ----

-- 1) 현재 날짜와 타임스탬프 추출하기
select current_date() as dt
, current_timestamp() as stamp;

-- 2) 지정한 값의 날짜/시각 데이터 추출하기
select date('2023-01-13') as dt
, timestamp('2023-01-13 15:45:00') as stamp;

-- 3) 날짜/시각에서 특정 필드 추출하기 - 타임스탬프의 자료형에서 연월일 추출
select stamp
, extract(year from stamp) as year
, extract(month from stamp) as month
, extract(day from stamp) as day
, extract(hour from stamp) as hour
from (select cast('2023-01-13 15:45:00' as timestamp) as stamp) as t; --cast-as함수를 사용하여 하나의 유형에서 다른 유형으로 데이터 형식을 변환한다.

-- 4) 날짜/시각에서 특정 필드 추출하기 - 타임스탬프를 나타내는 문자열에서 연, 월, 일을 추출하는 쿼리

select stamp
, substr(stamp, 1, 4) as year
, substr(stamp, 6, 2) as month
, substr(stamp, 9, 2) as day
, substr(stamp, 12, 2) as hour
, substr(stamp, 1, 7) as year_month
from (select cast('2023-01-13 15:45:00' as string) as stamp) as t;



---- 5_5. 결측 값을 디폴트 값으로 대치하기 ----

-- 테이블 만들기 : 쿠폰 사용 여부가 함께 있는 구매 로그 테이블
DROP TABLE IF EXISTS inlaid-lane-373607.sqlr202301.lec5_5;
CREATE TABLE inlaid-lane-373607.sqlr202301.lec5_5(
    purchase_id string(255)
  , amount      integer
  , coupon      integer
);

INSERT INTO inlaid-lane-373607.sqlr202301.lec5_5
VALUES
    ('10001', 3280, NULL)
  , ('10002', 4650,  500)
  , ('10003', 3870, NULL)
;

-- 1) 구매액에서 할인 쿠폰 값을 제외한 매출 금액을 구하는 쿼리
select purchase_id, amount, coupon, amount-coupon as dc1, amount-coalesce(coupon, 0) as dc2  --dc1은 할인액이 null이므로 최종금액도 null로 표시되나, dc2는 coalesce함수를 사용해 coupon이 null 이면 그것을 0으로 변환하여 계산하기 때문에 숫자가 그대로 표시된다. 
from inlaid-lane-373607.sqlr202301.lec5_5;
