import os
from exif import Image
import argparse
import pathlib

EXTENSIONS = [".jpg", ".jpeg"]

def remove_exif_data_from_file(image_file) -> None:
    global args
    try:
        image = Image(image_file)
        if image.has_exif:
            if not args.silent:
                print(f"{image_file.name} contains EXIF information. Deleted.", flush=True)
            image.delete_all()
            image_file.close()
            image_file = open(image_file.name, "wb")
            image_file.write(image.get_file())
            image_file.close()
        else:
            if args.verbose:
                print(f"{image_file.name} does not contain any EXIF information.", flush=True)
        image_file.close()
    except Exception as e:
        if args.verbose:
            print(f"Error on file {image_file.name}", flush=True)
            print(e, flush=True)

parser = argparse.ArgumentParser(description="Removes EXIF data from JPEG image file")
parser.add_argument("-d", "--dir", type=pathlib.Path, help="Directory to walk recursively in order to remove exif data from JPEG files")
parser.add_argument("-f", "--file", type=argparse.FileType('rb'), help="JPEG File to remove exif data from")
parser.add_argument("-v", "--verbose", action="store_true", help="Print extra information")
parser.add_argument("-s", "--silent", action="store_true", help="Silent execution")
args = parser.parse_args()

if args.verbose and args.silent:
    parser.print_help()
    print("Silent and Verbose cannot be set at the same time")
    exit()

if args.file and args.dir:
    parser.print_help()
    print("Please choose between directory and file mode")
    exit()

if not args.file and not args.dir:
    parser.print_help()
    print("Please specify a file or a directory")
    exit()

if args.file:
    _, file_extension = os.path.splitext(args.file.name)
    print(file_extension)
    if not file_extension.lower() in EXTENSIONS:
        print(f"{args.file.name} is not a jpeg file.")
        parser.print_help()
        exit()
    remove_exif_data_from_file(args.file)
    exit()

for path, dirs, files in os.walk(args.dir):
    for filename in files:
        fullpath = os.path.join(path, filename)
        _, file_extension = os.path.splitext(filename)
        if file_extension.lower() in EXTENSIONS:
            image_file = open(fullpath, "rb")
            remove_exif_data_from_file(image_file)
            image_file.close()
