---- lecture5 응용하기
-- 테이블 만들기 및 값 삽입
CREATE TABLE inlaid-lane-373607.sqlr202301.lec5_adv(
  customer_id string(255)
  , reg_date date
  , num_of_purchase int64
  , recent_purchase_price1 int64
  , recent_purchase_price2 int64
  , recent_purchase_price3 int64
);

INSERT INTO inlaid-lane-373607.sqlr202301.lec5_adv
VALUES
    ('a001', '2022-04-24', 5, 16200, 55000, 34600)
  , ('a002', '2020-11-20', 34, 7450, 3300, 76500)
  , ('a003', '2021-08-19', 12, 175600, 1050, 6000)
  , ('a004', '2023-01-19', NULL, NULL, NULL, NULL)
  ;

 select * from inlaid-lane-373607.sqlr202301.lec5_adv;


-- 응용1) 회원들의 가입일자를 (reg_date) 추출하기
select customer_id, extract(year from reg_date) as year, extract(month from reg_date) as month, extract(day from reg_date) as day
from inlaid-lane-373607.sqlr202301.lec5_adv;

-- 응용2) a005 회원 추가하기
insert into inlaid-lane-373607.sqlr202301.lec5_adv values ('a005', '2022-10-06', 2, 56500, 372000, null);
select*from inlaid-lane-373607.sqlr202301.lec5_adv;

insert into inlaid-lane-373607.sqlr202301.lec5_adv (customer_id, reg_date) 
values ('a006', '2023-01-24');

delete inlaid-lane-373607.sqlr202301.lec5_adv
where customer_id='a006';


-- 응용3) 각 회원들의 총 구매액 구하기-첫 번째 방법 (null신경안씀)
select customer_id, recent_purchase_price1 + recent_purchase_price2 + recent_purchase_price3 as total_price
from inlaid-lane-373607.sqlr202301.lec5_adv;

-- 응용4) null을 0으로 대치하여 회원들의 총 구매액 구하기
select customer_id, coalesce(recent_purchase_price1,0) + coalesce(recent_purchase_price2,0) + coalesce(recent_purchase_price3,0)  
from inlaid-lane-373607.sqlr202301.lec5_adv;
