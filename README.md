import java.util.ArrayList;
import java.util.Scanner;

class User {
    private String username;
    private String password;

    public User(String username, String password) {
        this.username = username;
        this.password = password;
    }

    public String getUsername() {
        return username;
    }

    public String getPassword() {
        return password;
    }
}

class Reservation {
    private String name;
    private String date;
    private int numberOfGuests;
    private static int idCounter = 1;
    private int reservationId;

    public Reservation(String name, String date, int numberOfGuests) {
        this.name = name;
        this.date = date;
        this.numberOfGuests = numberOfGuests;
        this.reservationId = idCounter++;
    }

    public int getReservationId() {
        return reservationId;
    }

    @Override
    public String toString() {
        return "Reservation ID: " + reservationId + ", Name: " + name + ", Date: " + date + ", Guests: " + numberOfGuests;
    }
}

class ReservationSystem {
    private ArrayList<Reservation> reservations = new ArrayList<>();
    private ArrayList<User> users = new ArrayList<>();
    private User loggedInUser = null;

    public void registerUser(String username, String password) {
        users.add(new User(username, password));
        System.out.println("User registered successfully.");
    }

    public boolean loginUser(String username, String password) {
        for (User user : users) {
            if (user.getUsername().equals(username) && user.getPassword().equals(password)) {
                loggedInUser = user;
                System.out.println("Login successful.");
                return true;
            }
        }
        System.out.println("Invalid username or password.");
        return false;
    }

    public void logoutUser() {
        loggedInUser = null;
        System.out.println("Logged out successfully.");
    }

    public void makeReservation(String name, String date, int numberOfGuests) {
        if (loggedInUser == null) {
            System.out.println("Please log in to make a reservation.");
            return;
        }
        Reservation reservation = new Reservation(name, date, numberOfGuests);
        reservations.add(reservation);
        System.out.println("Reservation made successfully. Your reservation ID is " + reservation.getReservationId());
    }

    public void viewReservations() {
        if (loggedInUser == null) {
            System.out.println("Please log in to view reservations.");
            return;
        }
        if (reservations.isEmpty()) {
            System.out.println("No reservations found.");
        } else {
            for (Reservation reservation : reservations) {
                System.out.println(reservation);
            }
        }
    }

    public void cancelReservation(int reservationId) {
        if (loggedInUser == null) {
            System.out.println("Please log in to cancel a reservation.");
            return;
        }
        Reservation reservationToRemove = null;
        for (Reservation reservation : reservations) {
            if (reservation.getReservationId() == reservationId) {
                reservationToRemove = reservation;
                break;
            }
        }
        if (reservationToRemove != null) {
            reservations.remove(reservationToRemove);
            System.out.println("Reservation cancelled successfully.");
        } else {
            System.out.println("Reservation ID not found.");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        ReservationSystem reservationSystem = new ReservationSystem();
        while (true) {
            System.out.println("1. Register");
            System.out.println("2. Login");
            System.out.println("3. Make a reservation");
            System.out.println("4. View all reservations");
            System.out.println("5. Cancel a reservation");
            System.out.println("6. Logout");
            System.out.println("7. Exit");
            System.out.print("Choose an option: ");
            int choice = scanner.nextInt();
            scanner.nextLine();  // Consume newline

            switch (choice) {
                case 1:
                    System.out.print("Enter username: ");
                    String username = scanner.nextLine();
                    System.out.print("Enter password: ");
                    String password = scanner.nextLine();
                    reservationSystem.registerUser(username, password);
                    break;
                case 2:
                    System.out.print("Enter username: ");
                    username = scanner.nextLine();
                    System.out.print("Enter password: ");
                    password = scanner.nextLine();
                    reservationSystem.loginUser(username, password);
                    break;
                case 3:
                    System.out.print("Enter name: ");
                    String name = scanner.nextLine();
                    System.out.print("Enter date (YYYY-MM-DD): ");
                    String date = scanner.nextLine();
                    System.out.print("Enter number of guests: ");
                    int numberOfGuests = scanner.nextInt();
                    reservationSystem.makeReservation(name, date, numberOfGuests);
                    break;
                case 4:
                    reservationSystem.viewReservations();
                    break;
                case 5:
                    System.out.print("Enter reservation ID to cancel: ");
                    int reservationId = scanner.nextInt();
                    reservationSystem.cancelReservation(reservationId);
                    break;
                case 6:
                    reservationSystem.logoutUser();
                    break;
                case 7:
                    System.out.println("Exiting...");
                    scanner.close();
                    return;
                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        }
    }
}# Online-Reservations-system