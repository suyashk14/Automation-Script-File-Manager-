import os
import shutil

FILE_TYPES = {
    'Documents': ['.pdf', '.doc', '.docx', '.txt', '.xls', '.xlsx', '.ppt', '.pptx'],
    'Images': ['.jpg', '.jpeg', '.png', '.gif'],
    'Videos': ['.mp4', '.mov', '.avi', '.mkv'],
    'Music': ['.mp3', '.wav'],
    'Archives': ['.zip', '.rar'],
}

def get_category(extension):
    for category, extensions in FILE_TYPES.items():
        if extension.lower() in extensions:
            return category
    return 'Others'

def organize_by_type(base_path):
    summary = {}

    for root, dirs, files in os.walk(base_path):
        for file in files:
            file_path = os.path.join(root, file)
            _, ext = os.path.splitext(file)
            category = get_category(ext)
            dest_folder = os.path.join(base_path, category)
            os.makedirs(dest_folder, exist_ok=True)

            try:
                shutil.move(file_path, os.path.join(dest_folder, file))
                summary[category] = summary.get(category, 0) + 1
            except Exception:
                pass  # Skip if file already exists or error occurs

    print("\nSummary of Organized Files:")
    if summary:
        for category, count in summary.items():
            print(f"  ➤ {category}: {count} file(s)")
    else:
        print("  No files were moved.")

print("File Organizer")

folder_path = input("Enter the full folder path to organize: ").strip()

if not os.path.exists(folder_path):
    print("Folder not found. Please check the path and try again.")
else:
    organize_by_type(folder_path)
    print("\n File Organization completed!")
