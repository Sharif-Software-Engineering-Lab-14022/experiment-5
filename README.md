# experiment-5
پس از راه اندازی برنامه YourKit در Intellij به صورت زیر عمل میکنیم:
![image](https://github.com/Sharif-Software-Engineering-Lab-14022/experiment-5/assets/62210678/08ef750c-d23e-4252-b51d-099d19faa1cb)

پس از انتخاب این مورد برنامه آماده اجرا به صورت پروفایل خواهد بود و نرم افزار YourKit نیز مانیتور برنامه را شروع میکند. 

![image](https://github.com/Sharif-Software-Engineering-Lab-14022/experiment-5/assets/62210678/e819d143-7bb4-4a1d-9227-2cdbff802320)

پس از دادن اولین ورودی:
![image](https://github.com/Sharif-Software-Engineering-Lab-14022/experiment-5/assets/62210678/9ab6166a-9209-4729-b1d5-638ac17e9da8)

پس از دادن دومین ورودی:
![image](https://github.com/Sharif-Software-Engineering-Lab-14022/experiment-5/assets/62210678/06c4d3e1-17f0-433e-9190-78f943f9d8f1)

پس از دادن سومین ورودی:
![image](https://github.com/Sharif-Software-Engineering-Lab-14022/experiment-5/assets/62210678/503b9116-7cad-48f9-846e-46b1d0332ba7)

در کادر پایین عکس میبینیم تابع temp بیشترین استفاده (در انتهای نمودار) از cpu را داشته است. 
پس از بررسی کد متوجه می شویم که تابع Temp در برنامه هیچ نقشی ندارد. پس آن را حذف میکنیم و دوباره profiling را انجام میدهیم.

