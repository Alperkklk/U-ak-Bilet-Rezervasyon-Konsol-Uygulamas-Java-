import com.google.gson.Gson;
import com.google.gson.GsonBuilder;

import java.io.FileWriter;
import java.io.IOException;
import java.time.LocalDateTime;
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

// Aircraft Entity
class Aircraft {
    private String model;
    private String brand;
    private String serialNumber;
    private int seatCapacity;

    public Aircraft(String model, String brand, String serialNumber, int seatCapacity) {
        this.model = model;
        this.brand = brand;
        this.serialNumber = serialNumber;
        this.seatCapacity = seatCapacity;
    }

    public String getModel() { return model; }
    public String getBrand() { return brand; }
    public String getSerialNumber() { return serialNumber; }
    public int getSeatCapacity() { return seatCapacity; }

    @Override
    public String toString() {
        return brand + " " + model + " (SN: " + serialNumber + ", Seats: " + seatCapacity + ")";
    }
}

// Location Entity
class Location {
    private String country;
    private String city;
    private String airport;
    private boolean isActive;

    public Location(String country, String city, String airport, boolean isActive) {
        this.country = country;
        this.city = city;
        this.airport = airport;
        this.isActive = isActive;
    }

    public String getFullLocation() {
        return city + ", " + country + " - " + airport + (isActive ? " (Active)" : " (Inactive)");
    }

    public boolean isActive() {
        return isActive;
    }

    @Override
    public String toString() {
        return getFullLocation();
    }
}

// Flight Entity
class Flight {
    private Location location;
    private Aircraft aircraft;
    private LocalDateTime time;
    private int reservedSeats = 0;

    public Flight(Location location, Aircraft aircraft, LocalDateTime time) {
        this.location = location;
        this.aircraft = aircraft;
        this.time = time;
    }

    public boolean reserveSeat() {
        if (reservedSeats < aircraft.getSeatCapacity()) {
            reservedSeats++;
            return true;
        }
        return false;
    }

    public Location getLocation() { return location; }
    public Aircraft getAircraft() { return aircraft; }
    public LocalDateTime getTime() { return time; }

    @Override
    public String toString() {
        return "Flight to " + location + " at " + time + " with " + aircraft;
    }
}

// Reservation Entity
class Reservation {
    private String firstName;
    private String lastName;
    private int age;
    private Flight flight;

    public Reservation(String firstName, String lastName, int age, Flight flight) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.age = age;
        this.flight = flight;
    }

    @Override
    public String toString() {
        return firstName + " " + lastName + " (Age: " + age + ") reserved: " + flight;
    }
}

// DataManager
class DataManager {
    public static void saveReservationsToJson(List<Reservation> reservations, String filename) {
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        try (FileWriter writer = new FileWriter(filename)) {
            gson.toJson(reservations, writer);
            System.out.println("Reservations saved to " + filename);
        } catch (IOException e) {
            System.out.println("Error saving reservations: " + e.getMessage());
        }
    }
}

// Main Class
public class Main {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);

        Aircraft aircraft = new Aircraft("737", "Boeing", "SN12345", 5);
        Location location = new Location("Turkey", "Istanbul", "IST", true);
        Flight flight = new Flight(location, aircraft, LocalDateTime.of(2025, 6, 15, 14, 0));

        List<Reservation> reservations = new ArrayList<>();

        System.out.println("Available Flight:\n" + flight);

        while (true) {
            System.out.print("Reserve a seat? (y/n): ");
            String answer = input.nextLine();
            if (!answer.equalsIgnoreCase("y")) break;

            if (!flight.reserveSeat()) {
                System.out.println("No more seats available!");
                break;
            }

            System.out.print("First Name: ");
            String fname = input.nextLine();
            System.out.print("Last Name: ");
            String lname = input.nextLine();
            System.out.print("Age: ");
            int age = Integer.parseInt(input.nextLine());

            Reservation r = new Reservation(fname, lname, age, flight);
            reservations.add(r);
            System.out.println("Reservation successful: " + r);
        }

        DataManager.saveReservationsToJson(reservations, "reservations.json");
        input.close();
    }
}




















# ✈️ Uçak Bilet Rezervasyon Konsol Uygulaması (Java)

Bu proje, Java programlama dili ile Nesneye Dayalı Programlama (OOP) prensiplerine uygun olarak geliştirilmiş bir uçak bilet rezervasyon sistemidir. Konsol tabanlı bu uygulama sayesinde kullanıcılar uçuş bilgilerini görebilir ve koltuk kapasitesi uygun olduğu sürece rezervasyon yapabilir.

## 📦 İçerik

- Uçak Bilgileri (`Aircraft`)
- Lokasyon Bilgileri (`Location`)
- Uçuş Bilgileri (`Flight`)
- Rezervasyon İşlemleri (`Reservation`)
- JSON formatında veri kaydı (`DataManager`)

## 🛠️ Özellikler

- Konsoldan kullanıcı etkileşimi
- Uçuş detaylarının görüntülenmesi
- Koltuk kapasitesi kontrolü
- Başarılı rezervasyon sonrası verilerin `reservations.json` dosyasına kaydedilmesi

## 🔧 Gereksinimler

- Java 8 veya üzeri
- [Gson Kütüphanesi](https://mvnrepository.com/artifact/com.google.code.gson/gson)

Gson `.jar` dosyasını indirip projenize ekleyin veya Maven kullanıyorsanız `pom.xml` dosyasına şu satırı ekleyin:

```xml
<dependency>
    <groupId>com.google.code.gson</groupId>
    <artifactId>gson</artifactId>
    <version>2.10.1</version>
</dependency>
