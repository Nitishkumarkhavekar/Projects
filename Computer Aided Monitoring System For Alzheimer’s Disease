import cv2
import numpy as np
import matplotlib.pyplot as plt
import os
import tkinter as tk
from tkinter import filedialog

def clear_screen():
    os.system('cls' if os.name == 'nt' else 'clear')

def show_main_screen():
    while True:
        clear_screen()
        print("\nAlzheimer's Disease Progress Tracker")
        print("1. Alzheimer's Info")
        print("2. Calculate Progress")
        print("3. Exit")
        choice = input("Enter your choice: ")

        if choice == '1':
            show_alzheimer_info()
        elif choice == '2':
            check_progress()
        elif choice == '3':
            print("Exiting the application.")
            break
        else:
            print("Invalid choice! Try again.")

def show_alzheimer_info():
    clear_screen()
    print("\n### Alzheimer's Disease Information\n")
    print("1. Overview: Alzheimer's is a type of dementia that affects memory and thinking.")
    print("2. Symptoms: Memory loss, confusion, difficulty in problem-solving.")
    print("3. Causes: Genetic factors, aging, lifestyle.")
    print("4. Treatment: No cure, but medications can help manage symptoms.")
    input("\nPress Enter to return to the main menu...")

def preprocess_image(image_path):
    image = cv2.imread(image_path, cv2.IMREAD_GRAYSCALE)
    if image is None:
        print("Error: Unable to read image file.")
        return None
    image = cv2.GaussianBlur(image, (5, 5), 0)
    _, image_binary = cv2.threshold(image, 25, 255, cv2.THRESH_BINARY)
    return cv2.resize(image_binary, (300, 300))

def calculate_metrics(image):
    total_pixels = image.size
    white_pixels = np.sum(image == 255)
    black_pixels = total_pixels - white_pixels
    ratio = white_pixels / black_pixels if black_pixels > 0 else float('inf')
    return total_pixels, white_pixels, black_pixels, ratio

def check_progress():
    clear_screen()
    print("Select two MRI images to check progress.")

    root = tk.Tk()
    root.withdraw()
    root.attributes('-topmost', True)
    root.update()

    image1_path = filedialog.askopenfilename(title="Select MRI Image 1", filetypes=[("Image Files", "*.png;*.jpg;*.jpeg;*.bmp")])
    if not image1_path:
        print("No image selected.")
        root.destroy()
        input("Press Enter to return...")
        return

    image2_path = filedialog.askopenfilename(title="Select MRI Image 2", filetypes=[("Image Files", "*.png;*.jpg;*.jpeg;*.bmp")])
    if not image2_path:
        print("No image selected.")
        root.destroy()
        input("Press Enter to return...")
        return

    root.destroy()
    image1 = preprocess_image(image1_path)
    image2 = preprocess_image(image2_path)

    if image1 is None or image2 is None:
        input("Press Enter to return...")
        return

    metrics1 = calculate_metrics(image1)
    metrics2 = calculate_metrics(image2)
    progress_percentage = ((metrics2[1] - metrics1[1]) / metrics1[1]) * 100 if metrics1[1] != 0 else 0

    print(f"Progress in Percentage: {progress_percentage:.2f}%")
    print("You're recovering! Keep up the fight!" if progress_percentage < 0 else "Keep monitoring progress regularly.")

    fig, axes = plt.subplots(1, 2, figsize=(10, 5))
    axes[0].imshow(image1, cmap='gray')
    axes[0].set_title("Processed MRI Image 1")
    axes[0].axis('off')

    axes[1].imshow(image2, cmap='gray')
    axes[1].set_title("Processed MRI Image 2")
    axes[1].axis('off')
    plt.show()

    input("Press Enter to return...")

if __name__ == "__main__":
    show_main_screen()
