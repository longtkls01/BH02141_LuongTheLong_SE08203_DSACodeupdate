import java.util.ArrayList;
import java.util.Scanner;


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

public class Main {
  static ArrayList<Student> studentList = new ArrayList<>();
  static Scanner sc = new Scanner(System.in);

  public static void main(String[] args) {
    while (true) {
      System.out.println("\n===== STUDENT MANAGEMENT =====");
      System.out.println("1. Add Student");
      System.out.println("2. Edit Student");
      System.out.println("3. Delete Student");
      System.out.println("4. Search Student");
      System.out.println("5. Sort Students by Marks");
      System.out.println("6. Display All Students");
      System.out.println("0. Exit");
      System.out.print("Choose: ");

      int choice = -1;
      try {
        choice = Integer.parseInt(sc.nextLine().trim());
      } catch (Exception e) {
        System.out.println("Invalid input. Please enter a number from 0 to 6.");
        continue;
      }

      switch (choice) {
        case 1: addStudent(); break;
        case 2: editStudent(); break;
        case 3: deleteStudent(); break;
        case 4: searchStudent(); break;
        case 5: sortStudents(); break;
        case 6: displayAll(); break;
        case 0:
          System.out.println("Exiting program...");
          return;
        default: System.out.println("Invalid choice! Please select from the menu.");
      }
    }
  }

  // =========================================================
  // CÁC HÀM BẪY LỖI (VALIDATION METHODS)
  // =========================================================

  // Bẫy lỗi ID: Chỉ cho phép nhập SỐ
  static String getIdInput() {
    while (true) {
      String input = sc.nextLine().trim();
      if (input.isEmpty()) {
        System.out.print("ID cannot be empty! Try again: ");
      } else if (!input.matches("^[0-9]+$")) {
        // Regex kiểm tra chuỗi chỉ chứa các chữ số từ 0-9
        System.out.print("Invalid ID! Numbers only (No letters allowed). Try again: ");
      } else {
        return input;
      }
    }
  }

  // Bẫy lỗi Tên: Chỉ cho phép nhập CHỮ (Hỗ trợ cả Tiếng Việt) và KHOẢNG TRẮNG
  static String getNameInput() {
    while (true) {
      String input = sc.nextLine().trim();
      if (input.isEmpty()) {
        System.out.print("Name cannot be empty! Try again: ");
      } else if (!input.matches("^[\\p{L}\\s]+$")) {
        // Regex \p{L} hỗ trợ mọi chữ cái, \s là khoảng trắng
        System.out.print("Invalid name! Letters only (No numbers allowed). Try again: ");
      } else {
        return input; // Chuẩn hóa: loại bỏ khoảng trắng thừa ở 2 đầu
      }
    }
  }

  // Bẫy lỗi Điểm: Bắt buộc nhập số thập phân từ 0 đến 10
  static double getMarksInput() {
    while (true) {
      try {
        double marks = Double.parseDouble(sc.nextLine().trim());
        if (marks < 0 || marks > 10) {
          System.out.print("Marks must be between 0 and 10. Try again: ");
        } else {
          return marks;
        }
      } catch (NumberFormatException e) {
        System.out.print("Invalid marks! Please enter a valid number. Try again: ");
      }
    }
  }

  // Hàm hỗ trợ tìm sinh viên trả về object Student (trả về null nếu không thấy)
  static Student findStudentById(String id) {
    for (Student s : studentList) {
      if (s.id.equals(id)) {
        return s;
      }
    }
    return null;
  }

  // =========================================================
  // CÁC CHỨC NĂNG CHÍNH (CRUD)
  // =========================================================

  static void addStudent() {
    System.out.print("Enter ID: ");
    String id = getIdInput(); // Sử dụng hàm bẫy lỗi ID

    if (findStudentById(id) != null) {
      System.out.println("Error: ID already exists!");
      return;
    }

    System.out.print("Enter Name: ");
    String name = getNameInput(); // Sử dụng hàm bẫy lỗi Tên

    System.out.print("Enter Marks (0-10): ");
    double marks = getMarksInput(); // Sử dụng hàm bẫy lỗi Điểm

    studentList.add(new Student(id, name, marks));
    System.out.println("Student added successfully.");
  }

  static void editStudent() {
    System.out.print("Enter ID to edit: ");
    String id = getIdInput();

    Student s = findStudentById(id);
    if (s == null) {
      System.out.println("Student not found.");
      return;
    }

    System.out.print("Enter new Name: ");
    s.name = getNameInput();

    System.out.print("Enter new Marks (0-10): ");
    s.marks = getMarksInput();

    System.out.println("Student updated successfully.");
  }

  static void deleteStudent() {
    System.out.print("Enter ID to delete: ");
    String id = getIdInput();

    Student s = findStudentById(id);
    if (s != null) {
      studentList.remove(s);
      System.out.println("Student deleted successfully.");
    } else {
      System.out.println("Student not found.");
    }
  }

  static void searchStudent() {
    System.out.print("Enter ID to search: ");
    String id = getIdInput();

    Student s = findStudentById(id);
    if (s == null) {
      System.out.println("Student not found.");
    } else {
      System.out.println("\n--- Student Info ---");
      System.out.println("ID: " + s.id);
      System.out.println("Name: " + s.name);
      System.out.println("Marks: " + s.marks);
      System.out.println("Rank: " + s.getRank());
    }
  }

  static void sortStudents() {
    if (studentList.isEmpty()) {
      System.out.println("No students available to sort.");
      return;
    }

    // Sắp xếp TimSort của Java (Độ phức tạp O(n log n))
    studentList.sort((Student a, Student b) -> Double.compare(b.marks, a.marks));

    System.out.println("\n===== SORTED STUDENTS (DESCENDING MARKS) =====");
    for (Student s : studentList) {
      System.out.println(s.id + " | " + s.name + " | " + s.marks + " | " + s.getRank());
    }
  }

  static void displayAll() {
    if (studentList.isEmpty()) {
      System.out.println("No students available.");
      return;
    }

    System.out.println("\n===== STUDENT LIST =====");
    for (Student s : studentList) {
      System.out.println(s.id + " | " + s.name + " | " + s.marks + " | " + s.getRank());
    }
  }
}

