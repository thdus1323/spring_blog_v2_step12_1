# 배포 자동화

## 1. 배포 전 프로세스
- 익셉션 정의
- 로그남기기 + 로그를 영구저장 (logback, 직접DB에 기록(스케쥴), 외부라이브러리(sentry))
- 프로파일 설정 (mysql)
- 테이블 명령어 정리 (root로 접속해서)
- prod 모드로 로컬에 실행 (전체 다 실행 - 노가다)
- 배포 시작직전에 프로파일 dev로 변경해두기 (테스트해야되니까, GithubAction에서)

## 2. 배포 프로세스



```sql
create user 'metacoding'@'%' identified by 'metacoding1234';
create user 'metacoding'@'%' identified by 'metacoding1234';
GRANT ALL PRIVILEGES ON *.* TO 'metacoding'@'%';
create database blogdb;

use blogdb;

create table user_tb (
    id integer auto_increment,
    created_at timestamp(6),
    email varchar(255),
    password varchar(255),
    username varchar(255) unique,
    primary key (id)
) DEFAULT CHARSET=utf8mb4;

create table board_tb (
    id integer auto_increment,
    user_id integer,
    created_at timestamp(6),
    content varchar(255),
    title varchar(255),
    primary key (id),
    constraint fk_board_user
    foreign key (user_id) references user_tb(id)
) DEFAULT CHARSET=utf8mb4;

create table reply_tb (
    board_id integer,
    id integer auto_increment,
    user_id integer,
    created_at timestamp(6),
    comment varchar(255),
    primary key (id),
    constraint fk_reply_board
    foreign key (board_id) references board_tb(id),
    constraint fk_reply_user
    foreign key (user_id) references user_tb(id)
) DEFAULT CHARSET=utf8mb4;
```

