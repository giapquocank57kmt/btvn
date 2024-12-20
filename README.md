# btvn
# bai 1 hien thi nhiet do, do am, ap suat, thong so joystick dung flask

from flask import Flask

from sense_emu import SenseHat

import threading

import time

app = Flask(__name__)

sense = SenseHat()

# Flask route để trả về dữ liệu cảm biến

@app.route('/giapan')

def get_sensor_data():

    temperature = sense.get_temperature()
    
    humidity = sense.get_humidity()
    
    return f"Temperature: {temperature:.2f}°C, Humidity: {humidity:.2f}%"

# Hàm hiển thị dữ liệu trên Sense HAT

def display_sensor_data():

    while True:
        temperature = sense.get_temperature()
        humidity = sense.get_humidity()
        sense.show_message(f"T:{temperature:.1f}C H:{humidity:.1f}%", scroll_speed=0.05, text_colour=[255, 0, 0])
        time.sleep(2)  # Chờ 2 giây trước khi cập nhật

# Chạy Flask và hiển thị đồng thời

if __name__ == '__main__':

    # Tạo luồng riêng để chạy hiển thị dữ liệu
    display_thread = threading.Thread(target=display_sensor_data, daemon=True)
    display_thread.start()

    # Chạy Flask server
    
    app.run(host='0.0.0.0', port=5000)
