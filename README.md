<h2>بخش اول<h2>
پس از راه اندازی برنامه YourKit در Intellij به صورت زیر عمل میکنیم:

![image](https://github.com/Sharif-Software-Engineering-Lab-14022/experiment-5/assets/62210678/08ef750c-d23e-4252-b51d-099d19faa1cb)

پس از انتخاب این مورد برنامه آماده اجرا به صورت پروفایل خواهد بود و نرم افزار YourKit نیز مانیتور برنامه را شروع میکند. 

![image](https://github.com/Sharif-Software-Engineering-Lab-14022/experiment-5/assets/62210678/e819d143-7bb4-4a1d-9227-2cdbff802320)

پس از دادن سومین(آخرین) ورودی:

![image](https://github.com/Sharif-Software-Engineering-Lab-14022/experiment-5/assets/62210678/503b9116-7cad-48f9-846e-46b1d0332ba7)
![image](https://github.com/Sharif-Software-Engineering-Lab-14022/experiment-5/assets/62210678/1ef1b8c0-7a71-4e72-bf23-41dd5fe280bf)
![image](https://github.com/Sharif-Software-Engineering-Lab-14022/experiment-5/assets/62210678/e0153718-52e9-4643-9383-26b97924d4d8)


در کادر پایین عکس های بالا میبینیم تابع temp بیشترین استفاده (در انتهای نمودارها) از cpu و memory را داشته است. همچنین یک exception نیز رخ داده است.
پس از بررسی کد متوجه می شویم که تابع temp در برنامه هیچ نقشی ندارد. پس آن را حذف میکنیم و دوباره profiling را انجام میدهیم.

![image](https://github.com/Sharif-Software-Engineering-Lab-14022/experiment-5/assets/62210678/57c889fb-dda6-4fc4-b427-278d196ff902)
![image](https://github.com/Sharif-Software-Engineering-Lab-14022/experiment-5/assets/62210678/6e718f3e-7470-4c33-a181-a0f2482d19d0)
![image](https://github.com/Sharif-Software-Engineering-Lab-14022/experiment-5/assets/62210678/636c3f7a-8339-4f00-98ca-6a208ea067bf)

همان طور که در عکس بالا میبینیم، استفاده از cpu و memory کاهش یافته است و همچنین برنامه دچار exception نشده و به درستی اجرا می شود.
