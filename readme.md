# Extended AR HUD (IronVision)

## Overview
IronVision is a futuristic Augmented Reality Heads-Up Display (AR HUD) app inspired by Iron Man's AI systems. Built with Swift and RealityKit, it integrates various real-world data sources and utilities into an advanced AR experience for iOS devices. From GPS navigation and health tracking to face recognition and environmental alerts, IronVision provides a next-generation user experience.

## Features

### Core Functionalities
- **Time and Battery Display**: Stay informed of the current time and device battery level.
- **Face Recognition**: Identify users with Firebase integration and display their information in AR.
- **Messaging**: View and send messages directly within the AR interface.
- **GPS Navigation**: Get real-time latitude and longitude coordinates with location-based warnings (e.g., speed cameras).
- **Health Data**: Monitor heart rate and other health metrics using HealthKit integration.
- **Weather Information**: Fetch and display live weather data.

### Advanced Functionalities
- **Social Media Integration**: Fetch and display social media updates.
- **Traffic Alerts**: Receive warnings about nearby traffic conditions.
- **Music Controls**: Play or pause music directly from the AR HUD.
- **Calendar Events**: Display upcoming meetings or reminders.
- **Points of Interest (POI)**: Show nearby landmarks, restaurants, and more.
- **Object Recognition**: Identify objects in real-time using Vision framework.
- **Friend Notifications**: Detect and display nearby friends using GPS data.
- **Emergency Alerts**: Get critical warnings, such as weather hazards or low battery levels.

### Warning Indicators
- **Low Battery Alerts**: Receive a warning when your battery level drops below 20%.
- **Heart Rate Alerts**: Alerts for high heart rates (>120 BPM).
- **Environmental Hazards**: Warnings for unsafe zones (e.g., high pollution).
- **Speed Camera Warnings**: Notifications for nearby speed cameras.

## Installation
1. Clone the repository:
   ```bash
   git clone https://github.com/VadikGoyal1/IronVision.git
   ```
2. Open the project in Xcode:
   ```bash
   cd ironvision
   open IronVision.xcodeproj
   ```
3. Install dependencies:
   - Ensure Firebase is configured for your app.
   - Add HealthKit and CoreLocation permissions to your app.
4. Run the app on a compatible device (ARKit-supported iOS device required).

## Permissions
This app requires the following permissions:
- **Camera**: For AR and object recognition.
- **Location**: For GPS and POI features.
- **Health Data**: To display health metrics via HealthKit.
- **Notifications**: For alerts and reminders.

## Getting Started
### Prerequisites
Ensure you have the following:
- Xcode 14 or newer.
- iOS 15.0+ device with ARKit support.
- Firebase account for face recognition integration.

### Configuration
1. Set up Firebase:
   - Add the `GoogleService-Info.plist` file to the project.
   - Enable Firestore in your Firebase console.
2. Update `Info.plist`:
   - Add required keys for location, health, and camera permissions.
   - Example:
     ```xml
     <key>NSCameraUsageDescription</key>
     <string>Needed for AR features.</string>
     <key>NSLocationWhenInUseUsageDescription</key>
     <string>Needed for GPS-based features.</string>
     <key>NSHealthUpdateUsageDescription</key>
     <string>Needed for monitoring health metrics.</string>
     ```

## Usage
1. Launch the app on an ARKit-compatible device.
2. Grant required permissions when prompted.
3. Explore the AR HUD and interact with its features:
   - View real-time data displayed on the HUD.
   - Respond to alerts and warnings.
   - Use gesture-based controls for music, messages, and more.

## Contributing
Contributions are welcome! Follow these steps:
1. Fork the repository.
2. Create a new branch:
   ```bash
   git checkout -b feature-name
   ```
3. Commit your changes:
   ```bash
   git commit -m "Add feature-name"
   ```
4. Push to your branch:
   ```bash
   git push origin feature-name
   ```
5. Submit a pull request.

## Acknowledgments
- **Apple's ARKit and RealityKit**: For making AR development accessible.
- **Firebase**: For robust backend integration.
- **SwiftUI**: For the elegant and declarative UI framework.
- **OpenAI ChatGPT**: For code suggestions and documentation support.

---

Build your own IronVision today and experience the future of Augmented Reality!

