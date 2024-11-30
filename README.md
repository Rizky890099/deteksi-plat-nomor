# Install Tesseract dan OpenCV
!apt-get install -y tesseract-ocr
!apt-get install -y libtesseract-dev
!pip install pytesseract opencv-python

import urllib.request

# URL gambar plat nomor
url = "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQ1AwYf5yrDeKn0t-M2-X1673VCKKpdZIx7NQ&usqp=CAU"  # Gantilah dengan URL gambar Anda
image_path = "/content/plat_nomor.jpg"
# Download gambar
urllib.request.urlretrieve(url, image_path)

import cv2
import numpy as np
from matplotlib import pyplot as plt

# Load gambar
image = cv2.imread(image_path)

# Convert ke grayscale
gray_image = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

# Threshold untuk meningkatkan kontras
_, thresh_image = cv2.threshold(gray_image, 150, 255, cv2.THRESH_BINARY)

# Hilangkan noise menggunakan filter
denoised_image = cv2.medianBlur(thresh_image, 3)

# Tampilkan gambar hasil preprocessing
plt.figure(figsize=(10,10))
plt.subplot(1,3,1), plt.imshow(image[:,:,::-1]), plt.title("Original Image")
plt.subplot(1,3,2), plt.imshow(gray_image, cmap="gray"), plt.title("Grayscale Image")
plt.subplot(1,3,3), plt.imshow(denoised_image, cmap="gray"), plt.title("Denoised Image")
plt.show()

import pytesseract

# Mengatur path tesseract
pytesseract.pytesseract.tesseract_cmd = '/usr/bin/tesseract'

# Menggunakan Tesseract untuk mendeteksi teks dari gambar
text = pytesseract.image_to_string(denoised_image, config='--psm 8')

print("Hasil OCR: ", text)
