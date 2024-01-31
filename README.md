# AssignmentSDI
package basics;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class RegistrationManager 
{

	private static final String JDBC_URL = "jdbc:sqlite:registration.db";

    // Create operation
    public static void createRegistration(String name, String email, String dob) {
        try (Connection conn = DriverManager.getConnection(JDBC_URL);
             PreparedStatement stmt = conn.prepareStatement("INSERT INTO Registration (Name, Email, DateOfBirth) VALUES (?, ?, ?)")) {
            stmt.setString(1, name);
            stmt.setString(2, email);
            stmt.setString(3, dob);
            stmt.executeUpdate();
            System.out.println("Registration created successfully");
        } catch (SQLException e) {
            System.err.println("Error creating registration: " + e.getMessage());
        }
    }

    // Read operation
    public static void getRegistration(int id) {
        try (Connection conn = DriverManager.getConnection(JDBC_URL);
             PreparedStatement stmt = conn.prepareStatement("SELECT * FROM Registration WHERE ID = ?")) {
            stmt.setInt(1, id);
            ResultSet rs = stmt.executeQuery();
            if (rs.next()) {
                System.out.println("ID: " + rs.getInt("ID"));
                System.out.println("Name: " + rs.getString("Name"));
                System.out.println("Email: " + rs.getString("Email"));
                System.out.println("Date of Birth: " + rs.getString("DateOfBirth"));
            } else {
                System.out.println("Registration not found");
            }
        } catch (SQLException e) {
            System.err.println("Error getting registration: " + e.getMessage());
        }
    }

    // Update operation
    public static void updateRegistration(int id, String name, String email, String dob) {
        try (Connection conn = DriverManager.getConnection(JDBC_URL);
             PreparedStatement stmt = conn.prepareStatement("UPDATE Registration SET Name = ?, Email = ?, DateOfBirth = ? WHERE ID = ?")) {
            stmt.setString(1, name);
            stmt.setString(2, email);
            stmt.setString(3, dob);
            stmt.setInt(4, id);
            int rowsAffected = stmt.executeUpdate();
            if (rowsAffected > 0) {
                System.out.println("Registration updated successfully");
            } else {
                System.out.println("Registration not found");
            }
        } catch (SQLException e) {
            System.err.println("Error updating registration: " + e.getMessage());
        }
    }

    // Delete operation
    public static void deleteRegistration(int id) {
        try (Connection conn = DriverManager.getConnection(JDBC_URL);
             PreparedStatement stmt = conn.prepareStatement("DELETE FROM Registration WHERE ID = ?")) {
            stmt.setInt(1, id);
            int rowsAffected = stmt.executeUpdate();
            if (rowsAffected > 0) {
                System.out.println("Registration deleted successfully");
            } else {
                System.out.println("Registration not found");
            }
        } catch (SQLException e) {
            System.err.println("Error deleting registration: " + e.getMessage());
        }
    }

    public static void main(String[] args) {
        // Example usage
        createRegistration("John Doe", "john@example.com", "1990-01-01");
        getRegistration(1);
        updateRegistration(1, "Jane Doe", "jane@example.com", "1995-05-05");
        deleteRegistration(1);
    }
}

	
	

