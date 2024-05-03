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
            new Student(name.toString(), getHash(password.toString()), age, grade);

            if (i == allStudentsSize - 2 || i == allStudentsSize - 1)
                System.out.println("Student added: " + name.toString() + " " + password.toString() + " " + age + " " + grade);
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
            if (currentStudent.getName().equals(name) && currentStudent.password == getHash(password))
                return currentStudent;

            ++iterations;
            indexes.add(currentRandom);
        }


        if (iterations == maxIterations) {
            for (Student student : allStudents) {
                if (student.getName().equals(name) && student.password == getHash(password))
                    return student;
            }
        }

        return null;
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", grade=" + grade +
                '}';
    }
}
```
