## بخش اول:
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

## بخش دوم:
برای این بخش یک سیستم ذخیره و بازیابی ساده پیاده‌سازی شده است. در ابتدا کد این بخش به شرح زیر است:
```Java
import java.util.ArrayList;
import java.util.HashSet;
import java.util.Scanner;

public class Student {
    private static final ArrayList<Student> allStudents = new ArrayList<>();

    private final String name;
    private final int password;
    private final int age;
    private final int grade;

    public static void main(String[] args) {
        addInitialData();
        System.out.println("All students: " + allStudents.size());

        Scanner scanner = new Scanner(System.in);
        while (true) {
            System.out.println("Enter student credentials (name, password): ");
            String inputName = scanner.nextLine();
            String inputPassword = scanner.nextLine();
            if (inputName.equals("exit"))
                break;

            Student student = getStudent(inputName, inputPassword);
            System.out.println("Student: " + student);
        }
    }

    public Student(String name, int password, int age, int grade) {
        this.name = name;
        this.password = password;
        this.age = age;
        this.grade = grade;

        allStudents.add(this);
    }

    private static void addInitialData() {
        int allStudentsSize = 10000000;
        for (int i = 0; i < allStudentsSize; i++) {
            StringBuilder name = new StringBuilder();
            for (int j = 0; j < 10; j++)
                name.append((char) (Math.random() * 26 + 'a'));

            StringBuilder password = new StringBuilder();
            for (int j = 0; j < 10; j++)
                password.append((char) (Math.random() * 26 + 'a'));

            int age = (int) (Math.random() * 40);
            int grade = (int) (Math.random() * 20);
            Student student = new Student(name.toString(), getHash(password.toString()), age, grade);

            if (i == allStudentsSize - 2 || i == allStudentsSize - 1)
                System.out.println("Student added: " + student + " password: " + password);
        }
    }

    private static int getHash(String name) {
        int hash = 0;
        for (int i = 0; i < name.length(); i++) {
            hash = 31 * hash + name.charAt(i);
        }

        return hash;
    }

    public static Student getStudent(String name, String password) {
        HashSet<Integer> indexes = new HashSet<>();
        long maxIterations = 1000000000;
        long iterations = 0;
        while (indexes.size() < allStudents.size() && iterations < maxIterations) {
            int currentRandom = (int) (Math.random() * allStudents.size());
            Student currentStudent = allStudents.get(currentRandom);
            if (currentStudent.name.equals(name) && currentStudent.password == getHash(password))
                return currentStudent;

            ++iterations;
            indexes.add(currentRandom);
        }


        if (iterations == maxIterations) {
            for (Student student : allStudents) {
                if (student.name.equals(name) && student.password == getHash(password))
                    return student;
            }
        }

        return null;
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", password=" + password +
                ", age=" + age +
                ", grade=" + grade +
                '}';
    }
}
```

در ابتدای اجرای کد، یک سری داده‌ی fake به درون سیستم اضافه می‌کنیم. تمام دانشجویان در یک آرایه‌ی static ذخیرخ می‌شوند. همچنین اطلاعات دو دانشجوی آخر برای دسترسی و چک کردن سیستم، در console چاپ می‌شوند. برای name و password این داده‌ها یک سری داده‌ی رندوم استفاده شده است. توجه کنید که passwordها به صورت hash شده درون سیستم ذخیره می‌شوند. سپس سیستم از کاربر می‌خواهد که name و password یک کاربر را وارد کند و سیستم اطلاعات آن کاربر را خروجی می‌دهد. تا وقتی که کاربر دستور exit را وارد نکند، این روند گرفتن name و password ادامه می‌یابد. روند بازیابی داده‌ی شخض مورد نظر به شکل زیر است:
    یک میلیارد بار عدد تصادفی انتخاب می‌شود. هر بار می‌بینیم که آیا در آن index از عدد انتخابی در آرایه‌ی تمام دانشجویان، دانشجوی انتخاب شده name و passwordاش با شخص مورد نظر یکسان است یا خیر. پس از یک میلیار بار اگر هنوز دانشجویی پیدا نشده باشد، به روی کل آرایه‌ی دانشجویان for می‌زنیم و آنگاه برابری name و password را چک می‌کنیم. در عکس‌های زیر می‌توانید ورودی و خروجی profile را مشاهده کنید:
```Bash
Student added: Student{name='vqsvieecyn', password=1032231955, age=28, grade=2} password: pqggzwzjrw
Student added: Student{name='pqpmhspmod', password=-1721416524, age=32, grade=5} password: fakjopafdx
All students: 10000000
Enter student credentials (name, password): 
12345
12345
Student: null
Enter student credentials (name, password): 
abcd
abcd
Student: null
Enter student credentials (name, password): 
vqsvieecyn
pqggzwzjrw
Student: Student{name='vqsvieecyn', password=1032231955, age=28, grade=2}
Enter student credentials (name, password): 
exit


Process finished with exit code 0
```

<img width="1680" alt="1) CPU" src="https://github.com/Sharif-Software-Engineering-Lab-14022/experiment-5/assets/79265203/78bf085e-98c9-4c72-90a7-587938ad7ed8">
<img width="1680" alt="2) Threads" src="https://github.com/Sharif-Software-Engineering-Lab-14022/experiment-5/assets/79265203/aa1995a5-7588-4e72-9f75-9b4a904efd24">
<img width="1680" alt="3) Deadlocks" src="https://github.com/Sharif-Software-Engineering-Lab-14022/experiment-5/assets/79265203/2e48e8ee-0f83-435a-b1c5-801b5586d022">
<img width="1680" alt="4) Memory" src="https://github.com/Sharif-Software-Engineering-Lab-14022/experiment-5/assets/79265203/0a279fc3-f139-4d67-aeb1-542713be55e1">
<img width="1680" alt="5) Exceptions" src="https://github.com/Sharif-Software-Engineering-Lab-14022/experiment-5/assets/79265203/c87b7979-6615-4093-b880-c6302807e813">
<img width="1680" alt="6) Telemetry" src="https://github.com/Sharif-Software-Engineering-Lab-14022/experiment-5/assets/79265203/1686992e-74d4-463d-ace7-de5df8b96ff3">
<img width="1680" alt="7) Summary" src="https://github.com/Sharif-Software-Engineering-Lab-14022/experiment-5/assets/79265203/233adfde-12fc-44d9-a771-155e0badf2ed">
