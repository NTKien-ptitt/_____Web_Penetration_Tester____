### Mục Lục (Table of Contents)

- [1. Introduction](#Introduction) *(Giới thiệu)*  
- [2. Terminology](#Terminology) *(Thuật ngữ)*  
  - [2.1. JSON Web Token (JWT)](#Json-web-token-jwt) *(Mã thông báo Web JSON)*  
  - [2.2. Claim](#Claim) *(Yêu cầu/Khẳng định)*  
  - [2.3. JWT Claims Set](#Jwt-claims-set) *(Tập hợp các yêu cầu JWT)*  
  - [2.4. Nested JWT](#Nested-jwt) *(JWT lồng ghép)*  
  - [2.5. Unsecured JWT](#Unsecured-jwt) *(JWT không bảo mật)*  
  - [2.6. NumericDate](#Numericdate) *(Kiểu ngày số)*    
- [3. JSON Web Token (JWT) Overview](#Json-web-token-jwt-overview) *(Tổng quan về JSON Web Token)*  
  - [3.1. Example JWT](#Example-jwt) *(Ví dụ JWT)*  
- [4. JWT Claims](#Jwt-claims) *(Yêu cầu JWT)*  
  - [4.1. Registered Claim Names](#registered-claim-names) *(Tên yêu cầu được đăng ký)*  
    - [4.1.1. "iss" (Issuer)](#iss-issuer) *(Người phát hành)*  
    - [4.1.2. "sub" (Subject)](#sub-subject) *(Chủ thể)*  
    - [4.1.3. "aud" (Audience)](#aud-audience) *(Khán giả)*  
    - [4.1.4. "exp" (Expiration Time)](#exp-expiration-time) *(Thời gian hết hạn)*  
    - [4.1.5. "nbf" (Not Before)](#nbf-not-before) *(Không trước thời điểm)*  
    - [4.1.6. "iat" (Issued At)](#iat-issued-at) *(Thời điểm phát hành)*  
    - [4.1.7. "jti" (JWT ID)](#jti-jwt-id) *(ID JWT)*  
  - [4.2. Public Claim Names](#public-claim-names) *(Tên yêu cầu công khai)*  
  - [4.3. Private Claim Names](#private-claim-names) *(Tên yêu cầu riêng tư)*  
- [5. JOSE Header](#jose-header) *(Phần đầu JOSE)*  
  - [5.1. "typ" (Type) Header Parameter](#typ-type-header-parameter) *(Tham số tiêu đề "typ" - Loại)*  
  - [5.2. "cty" (Content Type) Header Parameter](#cty-content-type-header-parameter) *(Tham số tiêu đề "cty" - Loại nội dung)*  
  - [5.3. Replicating Claims as Header Parameters](#replicating-claims-as-header-parameters) *(Nhân đôi yêu cầu dưới dạng tham số tiêu đề)*  
- [6. Unsecured JWTs](#unsecured-jwts) *(JWT không bảo mật)*  
  - [6.1. Example Unsecured JWT](#example-unsecured-jwt) *(Ví dụ JWT không bảo mật)*  
- [7. Operations with JWTs](#operations-with-jwts) *(Thao tác với JWTs)*  
  - [7.1. Tạo JWT (Creating a JWT)](#creating-a-jwt)  
  - [7.2. Xác thực JWT (Validating a JWT)](#validating-a-jwt)  
  - [7.3. Quy tắc so sánh chuỗi (String Comparison Rules)](#string-comparison-rules)  
- [8. Implementation Requirements](#implementation-requirements) *(Yêu cầu triển khai)*  
  - [8.1. Thuật toán ký số và MAC (Signature and MAC Algorithms)](#signature-and-mac-algorithms)  
  - [8.2. Hỗ trợ JWT được mã hóa (Encrypted JWTs)](#encrypted-jwts)  
  - [8.3. JWT lồng ghép (Nested JWTs)](#nested-jwts)  
- [9. URI for Declaring that Content is a JWT](#uri-for-declaring-that-content-is-a-jwt) *(URI để khai báo nội dung là JWT)*  
- [10. IANA Considerations](#iana-considerations) *(Cân nhắc IANA)*  
  - [10.1. JSON Web Token Claims Registry](#json-web-token-claims-registry) *(Đăng ký yêu cầu JWT)*  
  - [10.2. Sub-Namespace Registration of urn:ietf:params:oauth:token-type:jwt](#sub-namespace-registration-of-urnietfparamsoauthtoken-typejwt) *(Đăng ký không gian con cho urn:ietf:params:oauth:token-type:jwt)*  
  - [10.3. Media Type Registration](#media-type-registration) *(Đăng ký loại phương tiện)*  
  - [10.4. Header Parameter Names Registration](#header-parameter-names-registration) *(Đăng ký tên tham số tiêu đề)*  
- [11. Security Considerations](#security-considerations) *(Cân nhắc bảo mật)*  
  - [11.1. Trust Decisions](#trust-decisions) *(Quyết định tin cậy)*  
  - [11.2. Signing and Encryption Order](#signing-and-encryption-order) *(Thứ tự ký và mã hóa)*  
- [12. Privacy Considerations](#privacy-considerations) *(Cân nhắc quyền riêng tư)*  

---

### **1. Introduction**  
- **JSON Web Token (JWT)** là một định dạng nhỏ gọn được thiết kế để biểu diễn thông tin xác nhận (*claims*), phù hợp với các môi trường bị giới hạn dung lượng, ví dụ:  
  - **HTTP Authorization headers** (dùng cho xác thực).  
  - **URI query parameters** (tham số truy vấn URL).  

- **Cấu trúc của JWT**:  
  - JWT gồm ba phần chính:  
    1. **Header**: Chứa thông tin thuật toán ký (như HS256) và loại token (thường là `"JWT"`).  
    2. **Payload**: Chứa các thông tin xác nhận (*claims*).  
    3. **Signature**: Được dùng để bảo vệ tính toàn vẹn của dữ liệu.  
  - JWT có thể được mã hóa (JWE) hoặc ký (JWS), giúp bảo vệ dữ liệu khỏi thay đổi trái phép hoặc truy cập bất hợp pháp.  

#### Ví dụ JWT:  
Một JWT đầy đủ có thể trông như sau (dạng chuỗi ký tự):  
```plaintext
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJqd3QuZXhhbXBsZSIsImV4cCI6MTYxNjIzOTAyMn0.dBjftJeZ4CVP-mB92K27uhbUJU1p1r_wW1gFWFOEjXk
```  

- **Giải thích các phần**:  
  - **Header**:  
    ```json
    {"alg": "HS256", "typ": "JWT"}
    ```
  - **Payload**:  
    ```json
    {"iss": "jwt.example", "exp": 1616239022}
    ```
  - **Signature**: Kết quả của việc mã hóa phần `header.payload` bằng thuật toán HMAC-SHA256 và khóa bí mật.

---

### **2. Terminology**  
#### **Các thuật ngữ quan trọng trong JWT**:  

1. **JSON Web Token (JWT)**  
   - Là chuỗi ký tự biểu diễn các thông tin xác nhận (*claims*), được ký hoặc mã hóa.  
   - Ví dụ:  
     ```plaintext
     eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
     ```

2. **Claim**  
   - Một thông tin được xác nhận về chủ thể, biểu diễn dưới dạng cặp **name/value**.  
   - **Claim Name**: Tên của thông tin.  
   - **Claim Value**: Giá trị của thông tin, có thể là chuỗi, số, hoặc JSON.  
   - Ví dụ:  
     ```json
     {"iss": "example.com", "exp": 1700000000}
     ```

3. **JWT Claims Set**  
   - Là một JSON object chứa tất cả các *claims*.  
   - Ví dụ:  
     ```json
     {
       "iss": "example.com",
       "sub": "user123",
       "exp": 1700000000
     }
     ```

4. **Nested JWT**  
   - JWT có thể được lồng nhau, ví dụ một JWT được ký (JWS) sau đó mã hóa (JWE).  
   - Ví dụ:  
     - **JWT lồng nhau**:  
       ```plaintext
       EncryptedJWT(SignedJWT(Header, Payload, Signature))
       ```

5. **Unsecured JWT**  
   - JWT không được bảo vệ bởi chữ ký hoặc mã hóa, thuật toán `"alg": "none"` được sử dụng.  
   - Ví dụ:  
     ```plaintext
     eyJhbGciOiJub25lIn0.eyJzdWIiOiIxMjM0NTY3ODkwIn0.
     ```

6. **NumericDate**  
   - Giá trị số JSON, biểu diễn thời gian tính từ **Epoch (1970-01-01T00:00:00Z UTC)**.  
   - Ví dụ:  
     - Thời gian **1700000000** tương ứng ngày 14/07/2023.  

---

---

### **3. JSON Web Token (JWT) Overview**  
JWT là cách biểu diễn một tập hợp các thông tin xác nhận (*claims*) dưới dạng một JSON object. Tập hợp này được mã hóa trong:  
- **JWS (JSON Web Signature)**: Dữ liệu được ký số hoặc bảo vệ bằng MAC.  
- **JWE (JSON Web Encryption)**: Dữ liệu được mã hóa.

#### **Cấu trúc**  
- JSON object chứa *claims* được gọi là **JWT Claims Set**.  
- JWT được biểu diễn thành chuỗi gồm các phần mã hóa **base64url** an toàn với URL, ngăn cách bởi dấu chấm (`.`).  
- Số phần phụ thuộc vào việc JWT là JWS hay JWE:  
  - **JWS**: Gồm `Header`, `Payload`, và `Signature`.  
  - **JWE**: Gồm `Header`, `Encrypted Key`, `IV`, `Ciphertext`, và `Authentication Tag`.

#### **Nested JWT**  
- JWT có thể lồng ghép (nested) để thực hiện đồng thời ký số và mã hóa, đảm bảo tính bảo mật tối đa.

---

### **3.1. Example JWT**  
Ví dụ về một JWT được ký số bằng **HMAC-SHA256**:  

#### **JOSE Header**  
```json
{"typ":"JWT", "alg":"HS256"}
```  
- Sau khi mã hóa **base64url**, ta được:  
  ```plaintext
  eyJ0eXAiOiJKV1QiLA0KICJhbGciOiJIUzI1NiJ9
  ```

#### **JWT Claims Set**  
```json
{"iss":"joe", "exp":1300819380, "http://example.com/is_root":true}
```  
- Sau khi mã hóa **base64url**, ta được:  
  ```plaintext
  eyJpc3MiOiJqb2UiLA0KICJleHAiOjEzMDA4MTkzODAsDQogImh0dHA6Ly9leGFtcGxlLmNvbS9pc19yb290Ijp0cnVlfQ
  ```

#### **Signature**  
- Sử dụng thuật toán HMAC-SHA256 để tạo chữ ký từ `Header` và `Payload`.  
- Kết quả chữ ký (base64url):  
  ```plaintext
  dBjftJeZ4CVP-mB92K27uhbUJU1p1r_wW1gFWFOEjXk
  ```

#### **Chuỗi JWT đầy đủ**  
```plaintext
eyJ0eXAiOiJKV1QiLA0KICJhbGciOiJIUzI1NiJ9.
eyJpc3MiOiJqb2UiLA0KICJleHAiOjEzMDA4MTkzODAsDQogImh0dHA6Ly9leGFtcGxlLmNvbS9pc19yb290Ijp0cnVlfQ.
dBjftJeZ4CVP-mB92K27uhbUJU1p1r_wW1gFWFOEjXk
```

---

### **4. JWT Claims**  
#### **Tập hợp Claims trong JWT**  
- Claims Set là một JSON object chứa các cặp *Claim Name* và *Claim Value*.  
- Quy tắc:  
  - **Claim Names** phải duy nhất. Nếu trùng, parser phải bỏ qua hoặc chỉ giữ giá trị cuối.  
  - JWT không hiểu claim nào sẽ bỏ qua claim đó.

#### **Phân loại Claims**  
1. **Registered Claim Names**: Các claim chuẩn được định nghĩa sẵn (không bắt buộc). Ví dụ:  
   - `iss` (Issuer): Ai phát hành token.  
   - `exp` (Expiration Time): Thời gian hết hạn.  
   - `aud` (Audience): Đối tượng nhận token.  

2. **Public Claim Names**:  
   - Các claim công khai, phải đăng ký với IANA để tránh xung đột.

3. **Private Claim Names**:  
   - Các claim riêng, do nhà phát triển định nghĩa. Cần cẩn thận vì có thể bị xung đột.

---

---

### **4.1. Registered Claim Names**  
Các *Claim Names* được đăng ký trong **IANA "JSON Web Token Claims" registry**.  
- **Mục đích**: Cung cấp một bộ *claim* phổ biến, có tính tương thích cao.  
- **Đặc điểm**:  
  - Không bắt buộc sử dụng trong mọi trường hợp.  
  - Tên ngắn gọn để giữ cho JWT nhỏ gọn.  

---

#### **4.1.1. "iss" (Issuer)**  
- **Ý nghĩa**: Xác định chủ thể phát hành JWT.  
- **Đặc điểm**:  
  - Giá trị là chuỗi phân biệt chữ hoa/chữ thường hoặc URI.  
  - Không bắt buộc sử dụng.  
- **Ví dụ**:  
  ```json
  {"iss": "example.com"}
  ```

---

#### **4.1.2. "sub" (Subject)**  
- **Ý nghĩa**: Xác định chủ thể mà JWT đang đề cập.  
- **Đặc điểm**:  
  - Giá trị phải duy nhất trong phạm vi của issuer hoặc toàn cầu.  
  - Giá trị là chuỗi phân biệt chữ hoa/chữ thường hoặc URI.  
  - Không bắt buộc sử dụng.  
- **Ví dụ**:  
  ```json
  {"sub": "user123"}
  ```

---

#### **4.1.3. "aud" (Audience)**  
- **Ý nghĩa**: Xác định đối tượng mà JWT dành cho.  
- **Đặc điểm**:  
  - Nếu đối tượng nhận JWT không khớp với giá trị trong `aud`, JWT sẽ bị từ chối.  
  - Giá trị có thể là một chuỗi hoặc mảng chuỗi (phân biệt chữ hoa/chữ thường).  
  - Không bắt buộc sử dụng.  
- **Ví dụ**:  
  ```json
  {"aud": ["example.com", "another-audience.com"]}
  ```

---

#### **4.1.4. "exp" (Expiration Time)**  
- **Ý nghĩa**: Thời điểm hết hạn của JWT.  
- **Đặc điểm**:  
  - Sau thời điểm này, JWT không còn được chấp nhận.  
  - Giá trị là số, đại diện số giây kể từ **Epoch time** (1970-01-01T00:00:00Z).  
  - Không bắt buộc sử dụng.  
- **Ví dụ**:  
  ```json
  {"exp": 1700000000} // 14/07/2023
  ```

---

#### **4.1.5. "nbf" (Not Before)**  
- **Ý nghĩa**: JWT không hợp lệ trước thời điểm này.  
- **Đặc điểm**:  
  - Đảm bảo JWT chỉ được sử dụng sau thời điểm chỉ định.  
  - Giá trị là số, đại diện số giây kể từ Epoch time.  
  - Không bắt buộc sử dụng.  
- **Ví dụ**:  
  ```json
  {"nbf": 1700000000}
  ```

---

#### **4.1.6. "iat" (Issued At)**  
- **Ý nghĩa**: Thời điểm phát hành JWT.  
- **Đặc điểm**:  
  - Giá trị là số, đại diện số giây kể từ Epoch time.  
  - Dùng để xác định độ tuổi của JWT.  
  - Không bắt buộc sử dụng.  
- **Ví dụ**:  
  ```json
  {"iat": 1699990000}
  ```

---

#### **4.1.7. "jti" (JWT ID)**  
- **Ý nghĩa**: ID duy nhất của JWT.  
- **Đặc điểm**:  
  - Được sử dụng để ngăn chặn việc JWT bị phát lại (replay).  
  - Giá trị là chuỗi phân biệt chữ hoa/chữ thường.  
  - Không bắt buộc sử dụng.  
- **Ví dụ**:  
  ```json
  {"jti": "unique-id-123"}
  ```

---

### **4.2. Public Claim Names**  
- Người dùng JWT có thể định nghĩa các *claim* công khai.  
- Để tránh xung đột tên:  
  - Đăng ký với **IANA JSON Web Token Claims registry**.  
  - Sử dụng tên có tính chất chống xung đột (Collision-Resistant Names).  

---

### **4.3. Private Claim Names**  
- **Ý nghĩa**: Các *claim* riêng do bên phát hành và bên nhận thỏa thuận.  
- **Đặc điểm**:  
  - Không cần đăng ký.  
  - Dễ xảy ra xung đột nếu không quản lý cẩn thận.  
- **Ví dụ**:  
  ```json
  {"custom-claim": "custom-value"}
  ```

---

---

### **5. JOSE Header**
**JOSE Header** (JSON Object Signing and Encryption Header) chứa thông tin về các thao tác mã hóa hoặc ký số được áp dụng lên JWT, và các thuộc tính bổ sung nếu cần.  
- Các quy tắc áp dụng cho JOSE Header phụ thuộc vào việc JWT là **JWS** hay **JWE**.

#### **5.1. "typ" (Type) Header Parameter**
- **Ý nghĩa**:  
  - Xác định loại media type của JWT (thường là `"JWT"`).  
  - Giúp phân biệt JWT với các đối tượng khác trong cùng cấu trúc dữ liệu.  
- **Khuyến nghị**:  
  - Giá trị nên là `"JWT"`, luôn viết hoa để tương thích với các hệ thống cũ.  
- **Tùy chọn**: Không bắt buộc sử dụng.

---

#### **5.2. "cty" (Content Type) Header Parameter**
- **Ý nghĩa**:  
  - Mô tả cấu trúc của JWT, đặc biệt trong trường hợp **Nested JWT** (JWT lồng nhau).  
- **Khuyến nghị**:  
  - Không nên sử dụng nếu không có lồng nhau.  
  - Nếu có lồng nhau, giá trị phải là `"JWT"`.  

---

#### **5.3. Replicating Claims as Header Parameters**
- Trong JWT mã hóa (JWE), một số ứng dụng có thể cần phiên bản không mã hóa của các *claims*.  
  - Ví dụ: `iss`, `sub`, `aud` có thể được lặp lại trong Header như là các tham số.  
- **Lưu ý**:  
  - Ứng dụng phải đảm bảo rằng chỉ các *claims* an toàn mới được sao chép sang Header mà không mã hóa.

---

### **6. Unsecured JWTs**  
JWT có thể không cần ký số hoặc mã hóa trong một số trường hợp (ví dụ khi bảo mật được đảm bảo bởi một lớp khác).  
- **Cách biểu diễn**:  
  - Giá trị `alg` trong Header được đặt là `"none"`.  
  - Chữ ký (`Signature`) là chuỗi trống (`""`).  

---

#### **6.1. Example Unsecured JWT**
- **JOSE Header**:  
  ```json
  {"alg": "none"}
  ```
  Mã hóa **base64url**:  
  ```plaintext
  eyJhbGciOiJub25lIn0
  ```

- **JWT Claims Set**:  
  ```json
  {"iss": "joe", "exp": 1300819380, "http://example.com/is_root": true}
  ```
  Mã hóa **base64url**:  
  ```plaintext
  eyJpc3MiOiJqb2UiLA0KICJleHAiOjEzMDA4MTkzODAsDQogImh0dHA6Ly9leGFtcGxlLmNvbS9pc19yb290Ijp0cnVlfQ
  ```

- **Chữ ký**: Chuỗi trống (`""`).  

- **JWT hoàn chỉnh**:  
  ```plaintext
  eyJhbGciOiJub25lIn0
  .
  eyJpc3MiOiJqb2UiLA0KICJleHAiOjEzMDA4MTkzODAsDQogImh0dHA6Ly9leGFtcGxlLmNvbS9pc19yb290Ijp0cnVlfQ
  .
  ```

### **Điểm quan trọng cần lưu ý**
1. **JOSE Header** xác định loại thao tác mật mã (ký hoặc mã hóa) được áp dụng.  
2. **Unsecured JWTs** chỉ nên được sử dụng nếu lớp bảo mật khác đảm bảo an toàn, ví dụ TLS.  
3. **Nested JWT** cần Header `cty` chỉ rõ JWT lồng nhau.  


---


---

### **7.1. Tạo JWT (Creating a JWT)**  
Để tạo JWT, thực hiện các bước sau (không yêu cầu thứ tự cụ thể nếu các bước độc lập):  
1. **Tạo JWT Claims Set**:  
   - Bao gồm các *claims* cần thiết.  
   - Cho phép có khoảng trắng, không cần chuẩn hóa trước khi mã hóa.  

2. **Chuyển thành UTF-8**:  
   - Chuyển JSON object của Claims Set thành chuỗi byte UTF-8 (gọi là `Message`).  

3. **Tạo JOSE Header**:  
   - Chứa các tham số Header cần thiết.  
   - Xác định JWT là **JWS** hoặc **JWE**.  

4. **Xử lý tùy theo loại JWT**:  
   - Nếu là **JWS**:  
     - Dùng `Message` làm payload và tạo JWS (ký số).  
   - Nếu là **JWE**:  
     - Dùng `Message` làm plaintext và tạo JWE (mã hóa).  

5. **Lồng JWT (nếu cần)**:  
   - Nếu cần lồng ghép, sử dụng JWT đã tạo làm `Message` mới, thêm tham số `"cty": "JWT"` vào Header và lặp lại từ bước 3.  

6. **Hoàn thành**:  
   - Kết quả cuối cùng là JWS hoặc JWE.  

---

### **7.2. Xác thực JWT (Validating a JWT)**  
Khi xác thực JWT, thực hiện các bước sau (JWT bị từ chối nếu bất kỳ bước nào thất bại):  

1. **Kiểm tra cấu trúc**:  
   - JWT phải chứa ít nhất một dấu chấm (`.`).  

2. **Giải mã JOSE Header**:  
   - Tách phần trước dấu chấm đầu tiên.  
   - Giải mã **base64url** để nhận JSON object của Header.  

3. **Kiểm tra Header**:  
   - Đảm bảo Header hợp lệ, chứa UTF-8 và JSON đúng quy cách.  
   - Chỉ chấp nhận tham số được hiểu hoặc được chỉ định bỏ qua.  

4. **Xác định loại JWT**:  
   - Xác định JWT là **JWS** hay **JWE**.  

5. **Xác thực loại JWT**:  
   - Nếu là **JWS**:  
     - Làm theo quy trình xác thực trong JWS.  
     - Giải mã **base64url** phần Payload để nhận `Message`.  
   - Nếu là **JWE**:  
     - Làm theo quy trình xác thực trong JWE.  
     - Giải mã để nhận plaintext (là `Message`).  

6. **Kiểm tra lồng ghép**:  
   - Nếu Header có `"cty": "JWT"`, `Message` là JWT được lồng ghép. Lặp lại từ bước 1.  

7. **Giải mã Claims Set**:  
   - Giải mã **base64url** phần `Message`.  
   - Đảm bảo `Message` là JSON object hợp lệ, UTF-8.  

8. **Kiểm tra thuật toán**:  
   - Thuật toán trong JWT phải được ứng dụng chấp nhận.  
   - Nếu không, JWT bị từ chối ngay cả khi xác thực thành công.  

---

### **7.3. Quy tắc so sánh chuỗi (String Comparison Rules)**  
- So sánh tên và giá trị trong JSON object cần tuân theo **quy tắc JSON** từ [RFC 7159]:  
  - Phân biệt chữ hoa/chữ thường (case-sensitive).  
  - Các giá trị `"typ"` và `"cty"` có thể được so sánh khác nếu ứng dụng yêu cầu.  

- Ví dụ:  
  - `"alg"` (algorithm) phải được so sánh đúng từng ký tự.  
  - Tên miền trong `"iss"` có thể cần chuẩn hóa (ví dụ viết thường) nếu nhiều bên phải so sánh giá trị này.  

---

### **Ví dụ minh họa**  

#### **Tạo JWT (JWS)**  
- **Header**:  
  ```json
  {"alg": "HS256", "typ": "JWT"}
  ```  
- **Claims Set**:  
  ```json
  {"iss": "example.com", "exp": 1700000000}
  ```  
- **Chuỗi JWT (JWS)**:  
  ```plaintext
  eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.
  eyJpc3MiOiJleGFtcGxlLmNvbSIsImV4cCI6MTcwMDAwMDAwMH0.
  SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
  ```

#### **Xác thực JWT (JWS)**  
- Kiểm tra cấu trúc, Header, và chữ ký.  
- Giải mã Payload để kiểm tra giá trị `"iss"` và `"exp"`.  


---

### **8. Implementation Requirements**  
#### **Yêu cầu bắt buộc và tùy chọn khi triển khai JWT**  
1. **Thuật toán ký số và MAC (Signature and MAC Algorithms)**:  
   - **Bắt buộc**:  
     - HMAC SHA-256 (`HS256`).  
     - `none` (không ký).  
   - **Khuyến nghị**:  
     - RSASSA-PKCS1-v1_5 với SHA-256 (`RS256`).  
     - ECDSA với đường cong P-256 và SHA-256 (`ES256`).  
   - **Tùy chọn**: Các thuật toán khác và kích thước khóa lớn hơn.  

2. **Hỗ trợ JWT được mã hóa (Encrypted JWTs)**:  
   - **Tùy chọn**: Không bắt buộc cung cấp khả năng mã hóa.  
   - Nếu có hỗ trợ mã hóa, các thuật toán sau phải được triển khai:  
     - **RSAES-PKCS1-v1_5** với khóa 2048-bit (`RSA1_5`).  
     - **AES Key Wrap** với khóa 128 và 256-bit (`A128KW`, `A256KW`).  
     - **AES-CBC với HMAC SHA-2** (`A128CBC-HS256`, `A256CBC-HS512`).  
   - **Khuyến nghị**:  
     - **ECDH-ES** để thỏa thuận khóa (`ECDH-ES+A128KW`, `ECDH-ES+A256KW`).  
     - **AES-GCM** với khóa 128 và 256-bit (`A128GCM`, `A256GCM`).  

3. **JWT lồng ghép (Nested JWTs)**:  
   - Hỗ trợ là **tùy chọn**.

---

### **9. URI for Declaring that Content is a JWT**  
- Định danh chuẩn hóa cho nội dung JWT:  
  - **URN**: `"urn:ietf:params:oauth:token-type:jwt"`.  
- **Sử dụng**:  
  - Các ứng dụng cần khai báo loại nội dung sử dụng URI thay vì media type thông thường để xác định rằng nội dung là một JWT.

---

### **Điểm quan trọng**  
1. Hầu hết các thuật toán mã hóa và ký số bổ sung (như `RS256`, `ES256`) chỉ được **khuyến nghị**, không bắt buộc.  
2. Hỗ trợ mã hóa và lồng ghép JWT là **tùy chọn**, nhưng nếu triển khai, phải tuân theo các thuật toán chỉ định.  
3. URI `"urn:ietf:params:oauth:token-type:jwt"` giúp xác định rõ ràng nội dung là JWT trong hệ thống dùng URI để phân loại.

---

---

### 10. IANA Considerations

#### 10.1. JSON Web Token Claims Registry

- **Mục đích**: Tạo và quản lý danh sách các Claim Names cho JWT trong cơ sở dữ liệu của IANA. Mỗi Claim Name được ghi lại với mô tả và tham chiếu tài liệu kỹ thuật.

- **Quy trình đăng ký**:
   - Giá trị Claim được đăng ký theo nguyên tắc "Specification Required" sau khi được phê duyệt bởi các chuyên gia có thẩm quyền.
   - Quá trình đăng ký có một khoảng thời gian xem xét 3 tuần qua danh sách email **jwt-reg-review@ietf.org**.
   - Quyết định sẽ được phê duyệt hoặc từ chối, và yêu cầu chưa quyết định trong vòng 21 ngày có thể được chuyển đến **IESG** để giải quyết.

- **Ví dụ về các Claim Names ban đầu**:
   - **Claim Name**: `"iss"` (Issuer)
     - **Mô tả**: Người phát hành JWT.
     - **Tài liệu tham khảo**: Section 4.1.1 của RFC 7519
   - **Claim Name**: `"sub"` (Subject)
     - **Mô tả**: Chủ thể của JWT.
     - **Tài liệu tham khảo**: Section 4.1.2 của RFC 7519

- **Ví dụ code sử dụng Claims trong JWT**:
  ```javascript
  const jwt = require('jsonwebtoken');

  const token = jwt.sign({
    iss: 'myIssuer',
    sub: 'user123',
    aud: 'myApp'
  }, 'your-256-bit-secret', { expiresIn: '1h' });

  console.log(token);  // In ra token JWT đã được tạo
  ```

#### 10.2. Sub-Namespace Registration of urn:ietf:params:oauth:token-type:jwt

- **Mục đích**: Đăng ký giá trị `"token-type:jwt"` trong cơ sở dữ liệu "OAuth URI" của IANA, giúp nhận diện token là JWT trong các hệ thống OAuth.

- **Thông tin đăng ký**:
   - **URN**: `urn:ietf:params:oauth:token-type:jwt`
   - **Tên chung**: JSON Web Token (JWT) Token Type
   - **Thay đổi quản lý**: IESG
   - **Tài liệu tham khảo**: RFC 7519

- **Ví dụ code trong OAuth sử dụng JWT**:
  Giả sử bạn đang sử dụng JWT làm phần của quy trình OAuth:
  ```javascript
  const oauth2 = require('simple-oauth2').create({
    client: {
      id: 'your-client-id',
      secret: 'your-client-secret'
    },
    auth: {
      tokenHost: 'https://api.example.com'
    }
  });

  // Tạo một token JWT và sử dụng trong OAuth request
  const token = oauth2.accessToken.create({ access_token: 'your-jwt-token' });

  console.log(token.token.access_token);  // In ra JWT token
  ```

#### 10.3. Media Type Registration

- **Mục đích**: Đăng ký kiểu MIME `"application/jwt"` trong cơ sở dữ liệu "Media Types" của IANA, giúp xác định rằng nội dung là JWT.

- **Thông tin đăng ký**:
   - **Type name**: `application`
   - **Subtype name**: `jwt`
   - **Tài liệu tham khảo**: RFC 7519
   - **Ứng dụng sử dụng**: OpenID Connect, Mozilla Persona, Google, Salesforce, Amazon Web Services, v.v.
   - **Quản lý thay đổi**: IESG

- **Ví dụ code trong API trả về JWT**:
  Giả sử một API sử dụng kiểu MIME `"application/jwt"` khi trả về token JWT:
  ```javascript
  const express = require('express');
  const jwt = require('jsonwebtoken');

  const app = express();

  app.get('/api/token', (req, res) => {
    const payload = { iss: 'myIssuer', sub: 'user123' };
    const token = jwt.sign(payload, 'your-256-bit-secret', { expiresIn: '1h' });
    res.header('Content-Type', 'application/jwt');
    res.send(token);  // Trả về token JWT với kiểu MIME application/jwt
  });

  app.listen(3000, () => console.log('Server running on port 3000'));
  ```

#### 10.4. Header Parameter Names Registration

- **Mục đích**: Đăng ký các Header Parameters của JWT trong cơ sở dữ liệu "JSON Web Signature and Encryption Header Parameters" của IANA.

- **Thông tin đăng ký**:
   - **Header Parameter Name**: `"iss"` (Issuer)
     - **Mô tả**: Người phát hành JWT.
     - **Sử dụng trong**: JWE (JSON Web Encryption).
     - **Tài liệu tham khảo**: Section 4.1.1 của RFC 7519
   - **Header Parameter Name**: `"sub"` (Subject)
     - **Mô tả**: Chủ thể của JWT.
     - **Sử dụng trong**: JWE.
     - **Tài liệu tham khảo**: Section 4.1.2 của RFC 7519

- **Ví dụ code với Header Parameters trong JWE**:
  Khi bạn cần tạo JWT và gửi trong một tiêu đề HTTP, có thể sử dụng JWE để mã hóa:
  ```javascript
  const jose = require('node-jose');

  const payload = { iss: 'myIssuer', sub: 'user123' };
  const key = jose.JWK.asKey('your-256-bit-secret', 'oct');

  jose.JWS.createSign({ format: 'compact' }, key)
    .update(JSON.stringify(payload))
    .final()
    .then(token => {
      console.log('JWT Token: ', token);
    });
  ```

---

### 11. Security Considerations

- **Mục đích**: Các vấn đề bảo mật quan trọng đối với việc sử dụng JWT (JSON Web Token) liên quan đến việc bảo vệ khóa riêng của người dùng và các biện pháp phòng chống tấn công. Các vấn đề bảo mật trong các phần khác của JOSE (JWS, JWE, JWK) cũng áp dụng cho JWT.

#### Các Vấn Đề Bảo Mật Liên Quan:
- JWT phải được bảo mật và ký/ mã hóa để đảm bảo tính toàn vẹn và xác thực.
- Phải bảo vệ khóa riêng (private key) và khóa bí mật (secret key) trong quá trình sử dụng JWT/JWS/JWE.
- Các vấn đề bảo mật trong **JWS** và **JWE** cũng áp dụng cho JWT, đặc biệt là các vấn đề liên quan đến **so sánh Unicode** và các **vấn đề bảo mật của JSON**.

#### 11.1. Trust Decisions 
- **Vấn đề**: Nội dung của JWT không thể tin cậy được trong quyết định tin cậy nếu nó chưa được bảo mật và gắn kết với ngữ cảnh cần thiết.
- **Giải thích**: Để một JWT có thể được tin cậy, các khóa sử dụng để ký và/hoặc mã hóa JWT cần phải được xác minh là dưới sự kiểm soát của bên phát hành JWT. 
- **Ví dụ**: Nếu một JWT được sử dụng trong quy trình xác thực, cần đảm bảo rằng JWT đó được ký bằng khóa riêng của bên phát hành và có thể xác thực bằng khóa công khai.

  ```javascript
  const jwt = require('jsonwebtoken');
  
  // Khóa công khai (public key) và khóa riêng (private key)
  const publicKey = 'your-public-key';
  const privateKey = 'your-private-key';

  // Tạo JWT
  const payload = { sub: 'user123', iss: 'myIssuer' };
  const token = jwt.sign(payload, privateKey, { algorithm: 'RS256' });

  // Xác thực JWT
  jwt.verify(token, publicKey, (err, decoded) => {
    if (err) {
      console.log('JWT verification failed');
    } else {
      console.log('JWT verified successfully', decoded);
    }
  });
  ```

#### 11.2. Signing and Encryption Order 
- **Vấn đề**: Nếu cả ký và mã hóa JWT đều cần thiết, thứ tự ký và mã hóa cần phải chính xác để tránh các tấn công và đảm bảo tính bảo mật.
- **Khuyến nghị**: Thường thì JWT nên được ký trước và mã hóa sau. Điều này giúp bảo vệ chữ ký khỏi việc bị tách ra khỏi thông điệp mã hóa và đảm bảo tính bảo mật cho người ký.
- **Giải thích**: Nếu ký trước và mã hóa sau, bạn sẽ đảm bảo rằng chữ ký không thể bị bỏ đi mà chỉ có dữ liệu mã hóa sẽ được lộ. Điều này cũng bảo vệ quyền riêng tư của người ký.
  
  **Quá trình ký và mã hóa đúng**:
  1. Ký JWT.
  2. Mã hóa JWT đã ký.
  
  **Ví dụ** (Ký và mã hóa trong JWE):
  ```javascript
  const jose = require('node-jose');
  
  // Tạo payload
  const payload = { sub: 'user123', iss: 'myIssuer' };

  // Tạo khóa riêng và công khai
  const privateKey = jose.JWK.asKey('your-private-key', 'pem');
  const publicKey = jose.JWK.asKey('your-public-key', 'pem');

  // Ký JWT
  jose.JWS.createSign({ format: 'compact' }, privateKey)
    .update(JSON.stringify(payload))
    .final()
    .then(signedJWT => {
      // Mã hóa JWT đã ký
      jose.JWE.createEncrypt({ format: 'compact' }, publicKey)
        .update(signedJWT)
        .final()
        .then(encryptedJWT => {
          console.log('Encrypted JWT: ', encryptedJWT);
        });
    });
  ```

---

### 12. Privacy Considerations

- **Mục đích**: JWT có thể chứa thông tin nhạy cảm về quyền riêng tư. Do đó, cần phải có các biện pháp để ngăn chặn việc tiết lộ thông tin này cho các bên không mong muốn.

#### Các Biện Pháp Đảm Bảo Quyền Riêng Tư:
- **Mã hóa JWT**: Sử dụng JWT mã hóa để bảo vệ dữ liệu nhạy cảm.
- **Sử dụng TLS**: Nếu JWT chứa thông tin nhạy cảm chưa mã hóa, đảm bảo rằng nó được truyền qua các giao thức an toàn như **TLS** (Transport Layer Security) để bảo vệ dữ liệu khi truyền qua mạng.
- **Lựa chọn không chứa thông tin nhạy cảm**: Trong một số trường hợp, cách đơn giản nhất là không đưa thông tin nhạy cảm vào trong JWT.

- **Ví dụ về mã hóa JWT**:
  ```javascript
  const jose = require('node-jose');

  // Tạo payload chứa thông tin nhạy cảm
  const sensitiveData = { ssn: '123-45-6789', name: 'John Doe' };

  // Tạo khóa công khai và khóa riêng
  const privateKey = jose.JWK.asKey('your-private-key', 'pem');
  const publicKey = jose.JWK.asKey('your-public-key', 'pem');

  // Mã hóa thông tin nhạy cảm
  jose.JWE.createEncrypt({ format: 'compact' }, publicKey)
    .update(JSON.stringify(sensitiveData))
    .final()
    .then(encryptedJWT => {
      console.log('Encrypted JWT with sensitive data: ', encryptedJWT);
    });
  ```

---

