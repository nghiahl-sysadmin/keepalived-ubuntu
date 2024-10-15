### Kỹ thuật mã hóa Vigenère

#### Vigenère Cipher

**Bài toán 1:**
- Bản rõ \( M = \text{"TRUONG DAI HOC QUOC GIA"} \)
- Khoá \( K = \text{"HUYNH LE NGHIA"} \)

**Cách mã hoá:**
1. Lặp lại khóa \( K \) sao cho độ dài của nó bằng với độ dài của \( M \).
2. Thực hiện mã hoá từng ký tự của bản rõ với khóa theo quy tắc của Vigenère:
   \[
   C_i = (M_i + K_i) \mod 26
   \]
   Trong đó, \( C_i \) là ký tự mật mã, \( M_i \) là ký tự bản rõ, và \( K_i \) là ký tự của khóa.

**Ví dụ mã hoá:**
Giả sử:

- \( M = \text{TRUONG DAI HOC QUOC GIA} \) (không dấu, bỏ khoảng cách)
- \( K = \text{HUYNH LE NGHIAHUYNHLE NGH} \) (lặp khóa cho đủ độ dài)

Ta mã hoá từng ký tự:
- \( T + H \Rightarrow (19 + 7) \mod 26 = 0 \Rightarrow \text{A} \)
- \( R + U \Rightarrow (17 + 20) \mod 26 = 11 \Rightarrow \text{L} \)

Cứ tiếp tục như vậy cho đến hết bản rõ để nhận được ciphertext cuối cùng.

### Kỹ thuật mã hóa Playfair

#### Bài toán 2:
- Bản rõ \( M = \text{"THANH PHO HO CHI MINH"} \)
- Khoá \( K = \text{"HUYNH LE NGHIA"} \)

**Bước 1: Xây dựng ma trận khóa**

1. Loại bỏ các ký tự lặp trong khóa, bỏ qua các khoảng trống: `HUYN LE IG`
2. Viết các chữ cái còn lại theo thứ tự bảng chữ cái, bỏ chữ cái đã có trong khóa:
   ```
   H U Y N L
   E I G A B
   C D F K M
   O P Q R S
   T V W X Z
   ```

**Bước 2: Mã hoá bản rõ**
1. Chia bản rõ thành các cặp ký tự: `TH`, `AN`, `HP`, `HO`, `CH`, `IM`, `IN`, `H`
   - Nếu hai ký tự giống nhau hoặc số lẻ, thêm `X`: `TH`, `AN`, `HP`, `HO`, `CH`, `IM`, `IN`, `HX`
   
2. Mã hóa từng cặp:
   - **Cùng dòng:** đổi thành ký tự bên phải (xoay tròn)
   - **Cùng cột:** đổi thành ký tự bên dưới (xoay tròn)
   - **Khác dòng cột:** đổi ký tự thành giao điểm dòng và cột của hai ký tự

Ví dụ mã hóa từng cặp:
- `TH`: trong ma trận là `H` và `U`, khác dòng cột, đổi `UV`
- `AN`: khác dòng, đổi `NU`

Cứ tiếp tục để nhận chuỗi mã cuối.

**Bước 3: Giải mã**

Thực hiện ngược lại từng quy tắc: cùng dòng/cột hay khác dòng.

Tương tự, giả sử chuỗi mã là `UVNU..`, áp dụng ngược lại các quy tắc để giải mã từng cặp chữ cái về chuỗi bản rõ ban đầu
