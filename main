import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Scanner;

public class Main {
    static HashMap<String, Student> studentMap = new HashMap<>();
    static Scanner sc = new Scanner(System.in);

    public static void main(String[] args) {
        while (true) {
            System.out.println("\n===== STUDENT MANAGEMENT =====");
            System.out.println("1. Add Student");
            System.out.println("2. Edit Student");
            System.out.println("3. Delete Student");
            System.out.println("4. Search Student by ID (Hash Search)");
            System.out.println("5. Search Student by Name (Linear Search)");
            System.out.println("6. Sort Students by Marks (Bubble Sort)");
            System.out.println("7. Sort Students by Name (Selection Sort)");
            System.out.println("8. Display All Students");
            System.out.println("0. Exit");
            System.out.print("Choose: ");

            int choice = getIntInput(); // Sử dụng hàm có try...catch

            switch (choice) {
                case 1: addStudent(); break;
                case 2: editStudent(); break;
                case 3: deleteStudent(); break;
                case 4: searchStudentById(); break;
                case 5: searchStudentByName(); break;
                case 6: sortStudentsByMarks(); break;
                case 7: sortStudentsByName(); break;
                case 8: displayAll(); break;
                case 0:
                    System.out.println("Exiting...");
                    return;
                default:
                    System.out.println("Invalid choice! Please try again.");
            }
        }
    }

    // --- CÁC HÀM XỬ LÝ NHẬP LIỆU CÓ TRY...CATCH ---
    static int getIntInput() {
        while (true) {
            try {
                return Integer.parseInt(sc.nextLine());
            } catch (NumberFormatException e) {
                System.out.print("Invalid input! Please enter an integer: ");
            }
        }
    }

    static double getDoubleInput() {
        while (true) {
            try {
                return Double.parseDouble(sc.nextLine());
            } catch (NumberFormatException e) {
                System.out.print("Invalid input! Please enter a number: ");
            }
        }
    }

    // --- CÁC PHƯƠNG THỨC QUẢN LÝ ---
    static void addStudent() {
        System.out.print("Enter ID: ");
        String id = sc.nextLine();

        if (studentMap.containsKey(id)) {
            System.out.println("ID already exists!");
            return;
        }

        System.out.print("Enter Name: ");
        String name = sc.nextLine();

        System.out.print("Enter Marks: ");
        double marks = getDoubleInput(); // Dùng hàm bẫy lỗi

        studentMap.put(id, new Student(id, name, marks));
        System.out.println("Student added successfully.");
    }

    static void editStudent() {
        System.out.print("Enter ID to edit: ");
        String id = sc.nextLine();
        Student s = studentMap.get(id);

        if (s == null) {
            System.out.println("Student not found.");
            return;
        }

        System.out.print("Enter new Name: ");
        s.name = sc.nextLine();

        System.out.print("Enter new Marks: ");
        s.marks = getDoubleInput(); // Dùng hàm bẫy lỗi

        System.out.println("Student updated successfully.");
    }

    static void deleteStudent() {
        System.out.print("Enter ID to delete: ");
        String id = sc.nextLine();

        if (studentMap.remove(id) != null) {
            System.out.println("Student deleted.");
        } else {
            System.out.println("Student not found.");
        }
    }

    // THUẬT TOÁN SEARCH 1: Hash Search (Tìm theo Key của HashMap - Độ phức tạp O(1))
    static void searchStudentById() {
        System.out.print("Enter ID to search: ");
        String id = sc.nextLine();
        Student s = studentMap.get(id);

        if (s == null) {
            System.out.println("Student not found.");
        } else {
            System.out.println("Found -> ID: " + s.id + " | Name: " + s.name + " | Marks: " + s.marks + " | Rank: " + s.getRank());
        }
    }

    // THUẬT TOÁN SEARCH 2: Linear Search (Tìm kiếm tuyến tính theo Tên - Độ phức tạp O(n))
    static void searchStudentByName() {
        System.out.print("Enter Name to search: ");
        String keyword = sc.nextLine().toLowerCase();
        boolean found = false;

        System.out.println("\n===== SEARCH RESULTS =====");
        for (Student s : studentMap.values()) {
            if (s.name.toLowerCase().contains(keyword)) {
                System.out.println(s.id + " | " + s.name + " | " + s.marks + " | " + s.getRank());
                found = true;
            }
        }
        if (!found) {
            System.out.println("No student found with that name.");
        }
    }

    // THUẬT TOÁN SORT 1: Bubble Sort (Sắp xếp nổi bọt theo Điểm số giảm dần)
    static void sortStudentsByMarks() {
        List<Student> list = new ArrayList<>(studentMap.values());
        int n = list.size();

        for (int i = 0; i < n - 1; i++) {
            for (int j = 0; j < n - i - 1; j++) {
                if (list.get(j).marks < list.get(j + 1).marks) {
                    // Swap
                    Student temp = list.get(j);
                    list.set(j, list.get(j + 1));
                    list.set(j + 1, temp);
                }
            }
        }
        printList(list, "SORTED STUDENTS (DESCENDING MARKS - BUBBLE SORT)");
    }

    // THUẬT TOÁN SORT 2: Selection Sort (Sắp xếp chọn theo Tên sinh viên A-Z)
    static void sortStudentsByName() {
        List<Student> list = new ArrayList<>(studentMap.values());
        int n = list.size();

        for (int i = 0; i < n - 1; i++) {
            int minIdx = i;
            for (int j = i + 1; j < n; j++) {
                if (list.get(j).name.compareToIgnoreCase(list.get(minIdx).name) < 0) {
                    minIdx = j;
                }
            }
            // Swap
            Student temp = list.get(minIdx);
            list.set(minIdx, list.get(i));
            list.set(i, temp);
        }
        printList(list, "SORTED STUDENTS (A-Z NAME - SELECTION SORT)");
    }

    static void displayAll() {
        if (studentMap.isEmpty()) {
            System.out.println("No students available.");
            return;
        }
        printList(new ArrayList<>(studentMap.values()), "STUDENT LIST");
    }

    // Hàm hỗ trợ in danh sách
    static void printList(List<Student> list, String title) {
        System.out.println("\n===== " + title + " =====");
        for (Student s : list) {
            System.out.println(s.id + " | " + s.name + " | " + s.marks + " | " + s.getRank());
        }
    }
}

class Student {
    String id;
    String name;
    double marks;

    public Student(String id, String name, double marks) {
        this.id = id;
        this.name = name;
        this.marks = marks;
    }

    public String getRank() {
        if (marks < 5.0) return "Fail";
        else if (marks < 6.5) return "Medium";
        else if (marks < 7.5) return "Good";
        else if (marks < 9.0) return "Very Good";
        else return "Excellent";
    }
}
