from kivy.app import App
from kivy.uix.image import Image
from kivy.clock import Clock
from kivy.graphics.texture import Texture
import cv2
from ultralytics import YOLO

class CameraApp(App):
    def build(self):
        self.img = Image()
        self.capture = cv2.VideoCapture(0)
        self.model = YOLO("yolov8n.pt")
        Clock.schedule_interval(self.update, 1.0/30.0)
        return self.img

    def update(self, dt):
        ret, frame = self.capture.read()
        if ret:
            results = self.model(frame)
            frame = results[0].plot()

            buf = cv2.flip(frame, 0).tobytes()
            texture = Texture.create(size=(frame.shape[1], frame.shape[0]), colorfmt='bgr')
            texture.blit_buffer(buf, colorfmt='bgr', bufferfmt='ubyte')
            self.img.texture = texture

CameraApp().run()
