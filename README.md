# AdaptiveRTC: An AI-Driven Bandwidth Optimization Framework for WebRTC Video Streams

AdaptiveRTC is a real-time video transmission framework designed to dynamically optimize WebRTC streaming quality by adapting video bitrate and resolution according to changing network conditions. The system leverages WebRTC's built-in statistics API (getStats()) to monitor key Quality of Service (QoS) metrics in real time.

## Project Overview

The framework uses an initial rule-based (heuristic) controller that adjusts the encoder's maxBitrate parameter through RTCRtpSender.setParameters() to prevent quality degradation under poor network conditions or to improve visual quality when bandwidth permits. It includes an integrated data collection pipeline that logs per-second network, encoder, and stream metrics into structured CSV datasets for training machine learning models.

## Key Features

- **Real-Time Bitrate Adaptation**: Dynamically changes encoding bitrate based on live QoS metrics
- **Data Logging for ML Training**: Captures interval-based metrics (bitrate, loss, RTT, jitter, FPS, QP, etc.) for offline model training
- **EMA Smoothing**: Reduces noise in metrics to improve stability of control decisions
- **Configurable Network Profiles**: Tags each dataset with test conditions for reproducible experiments
- **Extensible to AI Control**: Designed to replace the heuristic controller with ML models
- **Cross-Platform Testing**: Works in desktop and mobile browsers under secure (HTTPS) contexts

## Getting Started

1. Clone the repository
2. Serve the project over HTTPS (required for WebRTC getUserMedia access):
   ```bash
   # Using Python
   python -m http.server 8000 --bind 127.0.0.1
   
   # Or using Node.js http-server
   npx http-server -p 8000
   ```
3. Open `https://localhost:8000` in your browser
4. Grant camera permissions when prompted

## How It Works

### Metrics Monitored
- **Round-Trip Time (RTT)**: Network latency in milliseconds
- **Packet Loss**: Percentage of lost packets
- **Jitter**: Variation in packet arrival times
- **Encoder Performance**: CPU usage, quantization parameters
- **Frame Rate**: Actual encoded/sent frames per second
- **Available Bandwidth**: Estimated outgoing bitrate capacity

### Adaptation Algorithm
The current heuristic controller uses exponential moving averages (EMA) to smooth metrics and applies these rules:
- **Decrease bitrate** if packet loss > 3% OR RTT > 250ms
- **Increase bitrate** if packet loss < 1% AND RTT < 120ms
- **Constraints**: Bitrate bounded between 150 kbps and 2.5 Mbps

## Usage

### Controls
- **Start/Stop Sampling**: Begin/end data collection
- **Download CSV**: Export collected metrics for analysis
- **Manual Bitrate**: Override automatic adaptation
- **Toggle Adaptation**: Enable/disable automatic bitrate control
- **Network Profile**: Tag data with test conditions

### Data Collection
The system logs comprehensive metrics every second:
- Network conditions (RTT, loss, jitter)
- Encoder statistics (bitrate, FPS, QP)
- Control decisions (bitrate changes)
- Session metadata (profile, timestamps)

## Research Applications

- **Adaptive Video Conferencing**: Optimize quality in unstable networks
- **Live Streaming Platforms**: AI-based quality optimization
- **QoE Research**: Network-aware media delivery studies
- **Educational/Telehealth**: Stable visual quality for critical applications

## Future AI Integration

The collected datasets enable training of machine learning models to:
- Predict optimal encoding parameters
- Replace heuristic rules with learned policies
- Implement reinforcement learning for adaptive control
- Develop predictive quality optimization

## Project Structure

```
AdaptiveRTC/
├── index.html          # Main application interface
├── README.md          # Project documentation
└── LICENSE           # Apache 2.0 license
```

## Technical Details

- **Framework**: Pure HTML5/JavaScript WebRTC implementation
- **Video Source**: Local camera (1280x720)
- **Connection**: Loopback peer connection for testing
- **Statistics**: Real-time getStats() monitoring
- **Export Format**: CSV with timestamped metrics

## Requirements

- Modern web browser with WebRTC support
- HTTPS context (for getUserMedia access)
- Camera permissions
- Stable network connection for meaningful testing

## License

This project is licensed under the Apache License 2.0 - see the [`LICENSE`](LICENSE) file for details.

## Contributing

Contributions are welcome! Please feel free to submit issues, feature requests, or pull requests to help improve AdaptiveRTC.

---

**Note**: This is a research prototype designed for academic and educational purposes. For production use, additional testing and security considerations may be required.