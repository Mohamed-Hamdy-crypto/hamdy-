import pytesseract
from pdf2image import convert_from_path
from PIL import Image

# مسار ملف PDF
pdf_path = "path_to_your_file.pdf"

# تحويل الصفحات إلى صور (نبدأ من الصفحة 11 = index 10)
images = convert_from_path(pdf_path, dpi=300)[10:]

# إعداد Tesseract للغة العربية
pytesseract.pytesseract.tesseract_cmd = r"path_to_tesseract_executable"  # فقط لو لم يكن في PATH
lang = 'ara'

full_text = ""
for i, img in enumerate(images, start=11):
    text = pytesseract.image_to_string(img, lang=lang)
    full_text += f"\n\n--- الصفحة {i} ---\n{text}"

# حفظ النص في ملف
with open("converted_text.txt", "w", encoding="utf-8") as f:
    f.write(full_text)
