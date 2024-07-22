import cv2
from PIL import Image, ImageDraw, ImageFont
import numpy as np

# Create an image with a red-black gradient background
width, height = 1920, 1080
gradient = np.zeros((height, width, 3), dtype=np.uint8)

for i in range(height):
    ratio = i / height
    red = int(255 * (1 - ratio))
    black = int(255 * ratio)
    gradient[i, :] = [red, 0, 0]

image = Image.fromarray(gradient, 'RGB')

# Add text "V12 TEAM" to the image
draw = ImageDraw.Draw(image)
font_size = 200
font = ImageFont.truetype("/usr/share/fonts/truetype/dejavu/DejaVuSans-Bold.ttf", font_size)
text = "V12 TEAM"
text_width, text_height = draw.textbbox((0, 0), text, font=font)[2:]
text_x = (width - text_width) / 2
text_y = (height - text_height) / 2

# Apply the text with black outline
outline_range = 5
for x in range(-outline_range, outline_range+1):
    for y in range(-outline_range, outline_range+1):
        draw.text((text_x + x, text_y + y), text, font=font, fill="black")

# Apply the text in white
draw.text((text_x, text_y), text, font=font, fill="white")

# Generate red, white, and black smoke effect
smoke_overlay = np.zeros((height, width, 3), dtype=np.uint8)
for i in range(100):
    # Generate random smoke patterns
    red_smoke = np.random.randint(0, 256, (height, width, 3), dtype=np.uint8) * [1, 0, 0]
    white_smoke = np.random.randint(0, 256, (height, width, 3), dtype=np.uint8) * [1, 1, 1]
    black_smoke = np.random.randint(0, 256, (height, width, 3), dtype=np.uint8) * [0, 0, 0]

    # Blend the smoke patterns together
    smoke_overlay = cv2.addWeighted(smoke_overlay, 0.9, red_smoke, 0.03, 0)
    smoke_overlay = cv2.addWeighted(smoke_overlay, 0.9, white_smoke, 0.03, 0)
    smoke_overlay = cv2.addWeighted(smoke_overlay, 0.9, black_smoke, 0.03, 0)

# Blend the smoke overlay with the original image
smoke_image = Image.blend(image, Image.fromarray(smoke_overlay, 'RGB'), alpha=0.5)

# Save the final image
output_path = "/mnt/data/V12_TEAM_image.png"
smoke_image.save(output_path)

output_path

