사용 기술 스택

- Java 11
- Spring boot 5.2.7.RELEASE
- Spring security 5.5.0
- Spring Framework
- MySQL 8.0
- Tomcat 9.0
- Sts 3.9.18
 






Mysql을 사용하여 DB설계

-- 사용자 정보 테이블
CREATE TABLE Users (
    UserID INT PRIMARY KEY AUTO_INCREMENT, -- 사용자 ID
    Username VARCHAR(20) NOT NULL, -- 사용자 이름
    Email VARCHAR(50) NOT NULL, -- 사용자 이메일
    Password VARCHAR(100) NOT NULL, -- 사용자 비밀번호
    ShippingAddress VARCHAR(1000) NOT NULL, -- 배송 주소
    ShippingPostalCode VARCHAR(20) NOT NULL, -- 배송 우편번호
    enabled char(1) default '0'
);

-- 카테고리 정보 테이블
CREATE TABLE Categories (
    CategoryID INT PRIMARY KEY AUTO_INCREMENT, -- 카테고리 ID
    CategoryName VARCHAR(50) NOT NULL -- 카테고리명
);

-- 제품 정보 테이블
CREATE TABLE Products (
    ProductID INT PRIMARY KEY AUTO_INCREMENT, -- 제품 ID
    ProductName VARCHAR(50) NOT NULL, -- 제품명
    Description TEXT, -- 제품 설명
    Price INT NOT NULL, -- 제품 가격
    Photo VARCHAR(255), -- 제품 사진 경로
    CategoryID INT, -- 외래 키: 카테고리 ID
    FOREIGN KEY (CategoryID) REFERENCES Categories(CategoryID)
);

-- 주문 정보 테이블
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY AUTO_INCREMENT, -- 주문 ID
    UserID INT, -- 주문한 사용자의 ID (외래키)
    ProductID INT, -- 주문한 제품의 ID (외래키)
    Quantity INT, -- 주문 수량
    ShippingAddress VARCHAR(1000) NOT NULL, -- 배송 주소
    ShippingPostalCode VARCHAR(20) NOT NULL, -- 배송 우편번호
    OrderDate TIMESTAMP DEFAULT CURRENT_TIMESTAMP, -- 주문 일자
    FOREIGN KEY (UserID) REFERENCES Users(UserID), -- 사용자 테이블과의 외래키 관계
    FOREIGN KEY (ProductID) REFERENCES Products(ProductID) -- 제품 테이블과의 외래키 관계
);

-- 결제 정보 테이블
CREATE TABLE Payments (
    PaymentID INT PRIMARY KEY AUTO_INCREMENT, -- 결제 ID
    OrderID INT, -- 주문 ID (외래키)
    PaymentMethod VARCHAR(50) NOT NULL, -- 결제 수단
    Amount INT NOT NULL, -- 결제 금액
    PaymentDate TIMESTAMP DEFAULT CURRENT_TIMESTAMP, -- 결제 일자
    Status VARCHAR(50), -- 결제 상태
    FOREIGN KEY (OrderID) REFERENCES Orders(OrderID) -- 주문 테이블과의 외래키 관계
);

CREATE TABLE LiveChat (
    ChatID INT PRIMARY KEY AUTO_INCREMENT, -- 채팅 ID
    UserID INT, -- 사용자 ID (외래키)
    Message TEXT, -- 채팅 메시지
    ChatTime TIMESTAMP DEFAULT CURRENT_TIMESTAMP, -- 채팅 시간
    ProductID INT, -- 제품 ID (외래키)
    FOREIGN KEY (UserID) REFERENCES Users(UserID), -- 사용자 테이블과의 외래키 관계
    FOREIGN KEY (ProductID) REFERENCES Products(ProductID) -- 제품 테이블과의 외래키 관계
);

-- 장바구니 테이블
CREATE TABLE Cart (
    CartID INT PRIMARY KEY AUTO_INCREMENT, -- 장바구니 ID
    UserID INT, -- 사용자 ID (외래키)
    ProductID INT, -- 제품 ID (외래키)
    Quantity INT, -- 제품 수량
    FOREIGN KEY (UserID) REFERENCES Users(UserID), -- 사용자 테이블과의 외래키 관계
    FOREIGN KEY (ProductID) REFERENCES Products(ProductID) -- 제품 테이블과의 외래키 관계
);

-- 카카오톡 결제 정보 테이블
CREATE TABLE KakaoPayments (
    PaymentID INT PRIMARY KEY AUTO_INCREMENT, -- 결제 ID
    OrderID INT, -- 주문 ID (외래키)
    PaymentMethod VARCHAR(50) NOT NULL, -- 결제 수단
    Amount INT NOT NULL, -- 결제 금액
    PaymentDate TIMESTAMP DEFAULT CURRENT_TIMESTAMP, -- 결제 일자
    KakaoTransactionID VARCHAR(100) NOT NULL, -- 카카오 거래 ID
    Status VARCHAR(50), -- 결제 상태
    FOREIGN KEY (OrderID) REFERENCES Orders(OrderID) -- 주문 테이블과의 외래키 관계
);

-- 이벤트 정보 테이블
CREATE TABLE Events (
    EventID INT PRIMARY KEY AUTO_INCREMENT, -- 이벤트 ID
    EventName VARCHAR(100) NOT NULL, -- 이벤트명
    Description TEXT, -- 이벤트 설명
    EventDate TIMESTAMP NOT NULL, -- 이벤트 일자
    Location VARCHAR(200) NOT NULL, -- 이벤트 장소
    Photo VARCHAR(255), -- 이벤트 사진 경로
    ProductID INT, -- 상품 ID (외래키)
    FOREIGN KEY (ProductID) REFERENCES Products(ProductID) -- 제품 테이블과의 외래키 관계
);

-- 후기 정보 테이블
CREATE TABLE Reviews (
    ReviewID INT PRIMARY KEY AUTO_INCREMENT, -- 후기 ID
    UserID INT, -- 사용자 ID (외래키)
    ProductID INT, -- 제품 ID (외래키)
    Rating INT NOT NULL, -- 평점
    Comment TEXT, -- 후기 내용
    ReviewDate TIMESTAMP DEFAULT CURRENT_TIMESTAMP, -- 후기 작성 일자
    FOREIGN KEY (UserID) REFERENCES Users(UserID), -- 사용자 테이블과의 외래키 관계
    FOREIGN KEY (ProductID) REFERENCES Products(ProductID) -- 제품 테이블과의 외래키 관계
);

CREATE TABLE tbl_users_auth (
    userid INT NOT NULL,
    auth VARCHAR(100) NOT NULL,
    CONSTRAINT fk_users_auth FOREIGN KEY (userid) REFERENCES Users(UserID)
);
CREATE TABLE LiveStreams (
    StreamID INT PRIMARY KEY AUTO_INCREMENT, -- 스트리밍 ID
    Title VARCHAR(100) NOT NULL, -- 제목
    VideoID VARCHAR(50) NOT NULL, -- 비디오 ID
    Description TEXT, -- 설명
    StartTime TIMESTAMP NOT NULL, -- 시작 시간
    EndTime TIMESTAMP, -- 종료 시간
    Photo VARCHAR(255), -- 사진의 경로를 저장할 열
    ProductID INT, -- 외래 키: 상품 ID
    ChatID INT, -- 외래 키: 채팅 ID
    FOREIGN KEY (ProductID) REFERENCES products(ProductID),
    FOREIGN KEY (ChatID) REFERENCES livechat(ChatID)
);

INSERT INTO Categories (CategoryName)
VALUES ('상의'),
       ('하의'),
       ('신발'),
       ('액세서리'),
       ('가방'),
       ('아우터'),
       ('전자기기'),
       ('가전제품'),
       ('서적'),
       ('가구');

 ![image](https://github.com/parkwooiu/project/assets/157097383/d40def1d-5ba7-4147-bba1-7bcb6778d387)

