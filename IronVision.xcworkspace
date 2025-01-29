import SwiftUI
import ARKit
import RealityKit
import AVFoundation
import Firebase
import CoreLocation
import Foundation
import HealthKit
import MapKit
import Vision

struct ContentView: View {
    var body: some View {
        ARViewContainer().edgesIgnoringSafeArea(.all)
    }
}

struct ARViewContainer: UIViewRepresentable {
    let locationManager = CLLocationManager()
    let healthStore = HKHealthStore()
    var audioPlayer: AVAudioPlayer?

    func makeUIView(context: Context) -> ARView {
        let arView = ARView(frame: .zero)

        // Start AR session
        let arConfig = ARFaceTrackingConfiguration()
        if ARFaceTrackingConfiguration.isSupported {
            arView.session.run(arConfig)
        } else {
            print("Face tracking not supported on this device.")
        }

        // Add HUD Elements
        let hudAnchor = AnchorEntity(.face)
        arView.scene.anchors.append(hudAnchor)

        // Time Display
        let timeText = createTextEntity(text: getCurrentTime(), position: [0, 0.2, -0.5])
        hudAnchor.addChild(timeText)

        // Battery Display
        let batteryText = createTextEntity(text: getBatteryLevel(), position: [0, 0.1, -0.5])
        hudAnchor.addChild(batteryText)

        // Face Recognition
        setupFaceRecognition(hudAnchor: hudAnchor)

        // Messaging Feature
        let messageText = createTextEntity(text: "Message: Hello!", position: [0, 0, -0.5])
        hudAnchor.addChild(messageText)

        // GPS Location
        setupGPS(hudAnchor: hudAnchor)

        // Social Media Integration
        setupSocialMediaData(hudAnchor: hudAnchor)

        // Health Data Integration
        setupHealthData(hudAnchor: hudAnchor)

        // Weather Information
        setupWeatherData(hudAnchor: hudAnchor)

        // Warning Indicators
        setupWarnings(hudAnchor: hudAnchor)

        // Music Controls
        setupMusicControls(hudAnchor: hudAnchor)

        // Calendar Integration
        setupCalendarEvents(hudAnchor: hudAnchor)

        // Traffic Alerts
        setupTrafficAlerts(hudAnchor: hudAnchor)

        // Fitness Tracking
        setupFitnessTracking(hudAnchor: hudAnchor)

        // Nearby Points of Interest
        setupPointsOfInterest(hudAnchor: hudAnchor)

        // Object Recognition
        setupObjectRecognition(hudAnchor: hudAnchor)

        // Nearby Friend Notifications
        setupNearbyFriendNotifications(hudAnchor: hudAnchor)

        // Emergency Alerts
        setupEmergencyAlerts(hudAnchor: hudAnchor)

        return arView
    }

    func updateUIView(_ uiView: ARView, context: Context) {}

    func createTextEntity(text: String, position: SIMD3<Float>) -> ModelEntity {
        let mesh = MeshResource.generateText(text, extrusionDepth: 0.01, font: .systemFont(ofSize: 0.1), alignment: .center)
        let material = SimpleMaterial(color: .green, isMetallic: true)
        let entity = ModelEntity(mesh: mesh, materials: [material])
        entity.position = position
        return entity
    }

    func getCurrentTime() -> String {
        let formatter = DateFormatter()
        formatter.dateFormat = "hh:mm a"
        return formatter.string(from: Date())
    }

    func getBatteryLevel() -> String {
        UIDevice.current.isBatteryMonitoringEnabled = true
        let batteryLevel = Int(UIDevice.current.batteryLevel * 100)
        return "Battery: \(batteryLevel)%"
    }

    func setupFaceRecognition(hudAnchor: AnchorEntity) {
        FirebaseApp.configure()

        let database = Firestore.firestore()
        database.collection("users").getDocuments { (querySnapshot, error) in
            if let error = error {
                print("Error fetching data: \(error)")
            } else {
                for document in querySnapshot!.documents {
                    let name = document.data()["name"] as? String ?? "Unknown"
                    let phone = document.data()["phone"] as? String ?? "Unknown"

                    let userInfoText = createTextEntity(text: "\(name) \n\(phone)", position: [0, -0.1, -0.5])
                    hudAnchor.addChild(userInfoText)
                }
            }
        }
    }

    func setupGPS(hudAnchor: AnchorEntity) {
        locationManager.requestWhenInUseAuthorization()

        if CLLocationManager.locationServicesEnabled() {
            locationManager.desiredAccuracy = kCLLocationAccuracyBest
            locationManager.startUpdatingLocation()

            if let location = locationManager.location {
                let latitude = location.coordinate.latitude
                let longitude = location.coordinate.longitude
                let locationText = createTextEntity(text: "Lat: \(latitude)\nLon: \(longitude)", position: [0, -0.2, -0.5])
                hudAnchor.addChild(locationText)

                // Example: Warning for Speed Camera
                if checkForSpeedCameraNearby(location: location) {
                    triggerWarning(hudAnchor: hudAnchor, message: "Speed Camera Ahead!")
                }
            }
        } else {
            let errorText = createTextEntity(text: "GPS not enabled", position: [0, -0.2, -0.5])
            hudAnchor.addChild(errorText)
        }
    }

    func checkForSpeedCameraNearby(location: CLLocation) -> Bool {
        // Placeholder function: Add actual logic for detecting nearby speed cameras
        return false
    }

    func setupSocialMediaData(hudAnchor: AnchorEntity) {
        let socialMediaText = createTextEntity(text: "Fetching Social Media Data...", position: [0, -0.3, -0.5])
        hudAnchor.addChild(socialMediaText)

        fetchTwitterData { twitterInfo in
            let twitterEntity = createTextEntity(text: twitterInfo, position: [0, -0.4, -0.5])
            hudAnchor.addChild(twitterEntity)
        }
    }

    func fetchTwitterData(completion: @escaping (String) -> Void) {
        DispatchQueue.global().asyncAfter(deadline: .now() + 2.0) {
            let fetchedData = "Twitter: @example_user"
            DispatchQueue.main.async {
                completion(fetchedData)
            }
        }
    }

    func setupHealthData(hudAnchor: AnchorEntity) {
        let healthTypes = Set([HKObjectType.quantityType(forIdentifier: .heartRate)!])

        healthStore.requestAuthorization(toShare: nil, read: healthTypes) { success, error in
            if success {
                let heartRateType = HKSampleType.quantityType(forIdentifier: .heartRate)!
                let query = HKSampleQuery(sampleType: heartRateType, predicate: nil, limit: 1, sortDescriptors: [NSSortDescriptor(key: HKSampleSortIdentifierStartDate, ascending: false)]) { _, samples, _ in
                    if let sample = samples?.first as? HKQuantitySample {
                        let heartRate = Int(sample.quantity.doubleValue(for: HKUnit(from: "count/min")))
                        let heartRateText = createTextEntity(text: "Heart Rate: \(heartRate) BPM", position: [0, -0.5, -0.5])
                        hudAnchor.addChild(heartRateText)
                    }
                }
                healthStore.execute(query)
            }
        }
    }

    func setupWeatherData(hudAnchor: AnchorEntity) {
        let weatherText = createTextEntity(text: "Fetching Weather Data...", position: [0, -0.6, -0.5])
        hudAnchor.addChild(weatherText)

        DispatchQueue.global().asyncAfter(deadline: .now() + 2.0) {
            let fetchedWeather = "Weather: 25°C, Clear"
            DispatchQueue.main.async {
                let weatherEntity = createTextEntity(text: fetchedWeather, position: [0, -0.7, -0.5])
                hudAnchor.addChild(weatherEntity)
            }
        }
    }

    func setupWarnings(hudAnchor: AnchorEntity) {
        if UIDevice.current.batteryLevel < 0.2 {
            triggerWarning(hudAnchor: hudAnchor, message: "Low Battery! Charge Soon.")
        }

        fetchHealthData { heartRate in
            if heartRate > 120 {
                triggerWarning(hudAnchor: hudAnchor, message: "High Heart Rate Detected!")
            }
        }

        setupEnvironmentalWarnings(hudAnchor: hudAnchor)
    }

    func fetchHealthData(completion: @escaping (Int) -> Void) {
        let heartRateType = HKSampleType.quantityType(forIdentifier: .heartRate)!
        let query = HKSampleQuery(sampleType: heartRateType, predicate: nil, limit: 1, sortDescriptors: [NSSortDescriptor(key: HKSampleSortIdentifierStartDate, ascending: false)]) { _, samples, _ in
            if let sample = samples?.first as? HKQuantitySample {
                let heartRate = Int(sample.quantity.doubleValue(for: HKUnit(from: "count/min")))
                completion(heartRate)
            }
        }
        healthStore.execute(query)
    }

    func setupMusicControls(hudAnchor: AnchorEntity) {
        let musicControlText = createTextEntity(text: "Music: Play/Pause", position: [0, -0.8, -0.5])
        hudAnchor.addChild(musicControlText)

        // Placeholder: Add actual music control logic here
    }

    func setupCalendarEvents(hudAnchor: AnchorEntity) {
        let calendarText = createTextEntity(text: "Fetching Calendar Events...", position: [0, -0.9, -0.5])
        hudAnchor.addChild(calendarText)

        DispatchQueue.global().asyncAfter(deadline: .now() + 2.0) {
            let calendarEvent = "Meeting at 3:00 PM"
            DispatchQueue.main.async {
                let calendarEventEntity = createTextEntity(text: calendarEvent, position: [0, -1.0, -0.5])
                hudAnchor.addChild(calendarEventEntity)
            }
        }
    }

    // Placeholder methods to handle other features
    func setupTrafficAlerts(hudAnchor: AnchorEntity) {}
    func setupFitnessTracking(hudAnchor: AnchorEntity) {}
    func setupPointsOfInterest(hudAnchor: AnchorEntity) {}
    func setupObjectRecognition(hudAnchor: AnchorEntity) {}
    func setupNearbyFriendNotifications(hudAnchor: AnchorEntity) {}
    func setupEmergencyAlerts(hudAnchor: AnchorEntity) {}

    func triggerWarning(hudAnchor: AnchorEntity, message: String) {
        let warningText = createTextEntity(text: message, position: [0, -0.3, -0.5])
        hudAnchor.addChild(warningText)
    }

    func setupEnvironmentalWarnings(hudAnchor: AnchorEntity) {}
}
