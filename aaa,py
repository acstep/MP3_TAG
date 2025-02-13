import os
from mutagen.flac import FLAC
from mutagen.mp3 import MP3

from mutagen.easyid3 import EasyID3
from zhconv import convert

def convert_id3_tags(directory):
    """
    將指定目錄及其子目錄下音樂檔案的 ID3 標籤簡體中文轉換為繁體中文，並詳細列出修改的標籤和值。

    Args:
        directory: 要處理的目錄路徑。
    """

    for root, _, files in os.walk(directory):
        for file in files:
            if file.endswith(('.flac', '.mp3', '.wav')):
                filepath = os.path.join(root, file)
                try:
                    if file.endswith('.flac'):
                        audio = FLAC(filepath)
                    elif file.endswith('.mp3'):
                        audio = MP3(filepath, ID3=EasyID3)
                    elif file.endswith('.wav'):
                        audio = WAV(filepath)
                    else:
                        continue  # 跳過不支援的檔案格式

                    print(f"處理檔案： {filepath}")

                    for tag_name in ['title', 'artist', 'album', 'genre']:
                        if tag_name in audio:
                            tag_value = audio[tag_name][0]
                            converted_value = convert(tag_value, 'zh-tw')
                            if converted_value != tag_value:
                                print(f"  - {tag_name}: {tag_value} -> {converted_value}")
                                audio[tag_name] = converted_value

                    audio.save()

                except Exception as e:
                    print(f"  - 錯誤：無法處理檔案 {filepath} - {e}")

if __name__ == "__main__":
    directory_path = input("請輸入要處理的目錄路徑：")
    if os.path.exists(directory_path):
        convert_id3_tags(directory_path)
    else:
        print("目錄不存在！")
