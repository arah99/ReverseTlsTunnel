
در این داکیومنت میخواهم نحوه اجرای برنامه به صورت سرویس رو توضیح بدم و اینکه چطور کاری کنیم هر n ساعت یک بار ری استارت بشه و همچنین موقع ری استارت شدن سیستم هم اجرا بشه

اول اینکه دستور سرور داخل و خارج اتون رو اجرا کنید و تست کنید تا مطمعن شین دستور ها درست هستن و کار میکنه بعد باید سرویس رو ایجاد کنیم

مرحله اول اینکه وارد این مسیر بشین
```sh
cd /etc/systemd/system
```
بعد باید سرویس رو ایجاد کنیم ؛ من اسم سرویس ام رو به اختیار میزارم tunnel
```sh
nano tunnel.service
```
خوب حالا این محتویات رو قرار میدیم 
```sh
[Unit]
Description= my tunnel service

[Service]
User=root
WorkingDirectory=/root
ExecStart=/root/RTT <your arguments> --terminate:24
Restart=always

[Install]
WantedBy=multi-user.target
```

خوب حالا دقت کنین که برنامه رو توی پوشه /root نصب کرده باشین فکر نکنم این نیاز به توضیح داشته باشه ؛ وارد پوشه روت بشین و یه بار دستور برنامه رو اجرا کنین تا فایل RTT اونجا باشه

دوم اینکه توی این فایلی که الان نوشتیم بخش ExecStart  باید جای \<your argemunts\>  پارامتر های برنامه رو بنویسید اما بعدش ما یک اپشن اضافه به اسم --terminate اضافه کردیم

این اپشن یه عدد میگیره و به ساعت هست که یعنی تو این مثال بعد از ۲۴ ساعت برنامه کامل بسته میشه

ولی چون ما سرویس ایجاد کردیم ؛ برنامه به محض اینکه بسته بشه به هر دلیلی ؛ دوباره توسط سیستم مجددا اجرا میشه
و اینطوری کاربر کمترین آسیپ رو میبینه اما خوب بازم خیلی بهتر هست که این ری استارت در زمانی انجام بشه مثلا ۴ تا ۸ صبح چون هربار 
ری استارت کانکشن ها قبلی رو لحظه ای قطع میکنه و اگه کاربر توی اون زمان مشفول دیدن ریلز اینستا یا یه سری برنامه های دیگه که به بسته نشدن کانکشن حساس هستن باشه ... ناراحت میشه :)
دیگه خودتون این زمان رو تنظیم کنین


خوب الان این دستورات رو به ترتیب اجرا میکنیم تا سرویس امون تکمیل بشه و در هنگام بوت شدن سیستم هم اجرا بشه

اول چک کنین برنامه در حال اجرا نباشه اگه اجرا بوده باید ببندینش با دستور 
> pkill RTT

بعد این مراحل رو اجرا کنید


> sudo systemctl daemon-reload

> sudo systemctl start tunnel.service

> sudo systemctl enable tunnel.service


اگه بعدا خواستین تونل رو استوپ کنین این دستور
> service tunnel stop

و یا

> sudo systemctl stop tunnel.service


