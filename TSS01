import tkinter as tk
from tkinter import filedialog
from PIL import Image, ImageTk
import cv2
import numpy as np
from ultralytics import YOLO

model = YOLO("yolov8n.pt")

class TrafficImageProcessorApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Traffic Image Processor")
        self.root.geometry("900x700")
        self.root.configure(bg="#F5F5F5")
        
        self.upload_button = tk.Button(root, text="Upload Image", command=self.upload_image, font=("Arial", 14))
        self.upload_button.pack(pady=10)
        
        self.canvas = tk.Label(root)
        self.canvas.pack()
        
        self.result_label = tk.Label(root, text="Traffic Density: 0 Vehicles", font=("Arial", 14))
        self.result_label.pack(pady=10)
        
        self.density_label = tk.Label(root, text="Density Level: Low", font=("Arial", 14))
        self.density_label.pack()
        
        self.density_bar = tk.Canvas(root, width=300, height=30, bg="white")
        self.density_bar.pack(pady=10)
        self.density_fill = self.density_bar.create_rectangle(0, 0, 0, 30, fill="green")
    
    def upload_image(self):
        file_path = filedialog.askopenfilename(filetypes=[("Image Files", "*.png;*.jpg;*.jpeg")])
        if not file_path:
            return
        self.process_image(file_path)
    
    def process_image(self, file_path):
        image = cv2.imread(file_path)
        image_resized = cv2.resize(image, (640, 480))
        results = model(image_resized)
        vehicle_count = sum(1 for obj in results[0].boxes.cls if obj in [2, 3, 5, 7])
        
        for box in results[0].boxes.xyxy:
            x1, y1, x2, y2 = map(int, box)
            cv2.rectangle(image_resized, (x1, y1), (x2, y2), (0, 255, 0), 2)
        
        image_rgb = cv2.cvtColor(image_resized, cv2.COLOR_BGR2RGB)
        img_pil = Image.fromarray(image_rgb)
        img_tk = ImageTk.PhotoImage(img_pil)
        
        self.canvas.config(image=img_tk)
        self.canvas.image = img_tk
        
        self.result_label.config(text=f"Traffic Density: {vehicle_count} Vehicles")
        self.update_density_scale(vehicle_count)
    
    def update_density_scale(self, count):
        max_vehicles = 20
        density_percentage = min(count / max_vehicles, 1.0)
        bar_width = int(300 * density_percentage)
        self.density_bar.coords(self.density_fill, 0, 0, bar_width, 30)
        
        if count <= 5:
            color, level = "green", "Low"
        elif count <= 10:
            color, level = "yellow", "Medium"
        else:
            color, level = "red", "High"
        
        self.density_bar.itemconfig(self.density_fill, fill=color)
        self.density_label.config(text=f"Density Level: {level}")

root = tk.Tk()
app = TrafficImageProcessorApp(root)
root.mainloop()
