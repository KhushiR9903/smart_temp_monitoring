#smart_temp_monitoring

import time
import random
import csv
from datetime import datetime

# Constants
TEMP_THRESHOLD = 30.0  # Celsius
LOG_FILE = 'temperature_log.csv'

# Simulate reading from a sensor (e.g., DHT11)
def read_temperature_humidity():
    temperature = round(random.uniform(20.0, 40.0), 2)
    humidity = round(random.uniform(30.0, 70.0), 2)
    return temperature, humidity

# Log data to a CSV file
def log_data(timestamp, temperature, humidity):
    with open(LOG_FILE, mode='a', newline='') as file:
        writer = csv.writer(file)
        writer.writerow([timestamp, temperature, humidity])

# Initialize log file with headers
def initialize_log():
    with open(LOG_FILE, mode='w', newline='') as file:
        writer = csv.writer(file)
        writer.writerow(['Timestamp', 'Temperature (C)', 'Humidity (%)'])

# Main monitoring loop
def monitor():
    initialize_log()
    print("Starting Smart Temperature Monitoring System...\n")
    try:
        while True:
            temperature, humidity = read_temperature_humidity()
            timestamp = datetime.now().strftime('%Y-%m-%d %H:%M:%S')
            log_data(timestamp, temperature, humidity)
            print(f"[{timestamp}] Temp: {temperature} °C | Humidity: {humidity} %")

            if temperature > TEMP_THRESHOLD:
                print("!! WARNING: Temperature threshold exceeded !!")

            time.sleep(2)  # Delay in seconds
    except KeyboardInterrupt:
        print("\nMonitoring stopped by user.")

if __name__ == '__main__':
    monitor()
