from PIL import Image, ImageDraw, ImageFont, ImageFilter
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
for x in range(-outline_range, outline_range + 1):
    for y in range(-outline_range, outline_range + 1):
        draw.text((text_x + x, text_y + y), text, font=font, fill="black")

# Apply the text in white
draw.text((text_x, text_y), text, font=font, fill="white")

# Create a smoke effect overlay
smoke_image = Image.new('RGB', (width, height), color=(0, 0, 0))
smoke_draw = ImageDraw.Draw(smoke_image)
for _ in range(100):
    x0 = np.random.randint(0, width)
    y0 = np.random.randint(0, height)
    x1 = x0 + np.random.randint(50, 200)
    y1 = y0 + np.random.randint(50, 200)
    color = np.random.choice([(255, 0, 0), (255, 255, 255), (0, 0, 0)])
    smoke_draw.ellipse([x0, y0, x1, y1], fill=color, outline=color)

smoke_image = smoke_image.filter(ImageFilter.GaussianBlur(50))

# Blend the smoke overlay with the original image
final_image = Image.blend(image, smoke_image, alpha=0.3)

# Save the final image
output_path = "/mnt/data/V12_TEAM_image.png"
final_image.save(output_path)

output_path
