======================================================================
                  CSE321 Lab Project: SimpleFS
                             Summer 2026
======================================================================

----------------------------------------------------------------------
1. IMPLEMENTATION OVERVIEW
----------------------------------------------------------------------
This project implements SimpleFS, an educational unmounted file system 
in C. It manages a 256 KiB binary image (disk.img) divided into 64 
fixed-size blocks (4096 bytes per block).

Key Components Implemented:
  - simplefs_builder.c: Formats a clean binary image by initializing 
    Block 0 (Superblock), Block 1 (Inode Bitmap), Block 2 (Data Bitmap), 
    Block 3 (Inode Table with Root Inode), and Block 4 (Root Directory 
    Data Block containing '.' and '..' entries).
  - simplefs_adder.c: Adds files to the image using first-fit 
    allocation for free inodes and data blocks. Updates bitmapped block 
    tracking, copies file contents (zero-padded to block boundaries), 
    creates root directory entries, and updates metadata accordingly.

----------------------------------------------------------------------
2. COMPILATION INSTRUCTIONS
----------------------------------------------------------------------
Compile both executable targets using standard C11 flags:

  gcc -Wall -Wextra -std=c11 simplefs_builder.c -o simplefs_builder
  gcc -Wall -Wextra -std=c11 simplefs_adder.c -o simplefs_adder

----------------------------------------------------------------------
3. EXECUTION EXAMPLES
----------------------------------------------------------------------
1. Create a fresh empty SimpleFS image:
     ./simplefs_builder --image disk.img

2. Add files from the working directory:
     ./simplefs_adder --input disk.img --file test1.txt
     ./simplefs_adder --input disk.img --file test2.txt

3. Inspect image structures via hexdump/xxd:
     xxd -l 128 disk.img                # Superblock verification
     xxd -s 4096 -l 8 disk.img          # Inode bitmap
     xxd -s 8192 -l 8 disk.img          # Data bitmap
     xxd -s 16384 -l 320 disk.img       # Root directory contents

----------------------------------------------------------------------
4. KNOWN LIMITATIONS & PROBLEMS
----------------------------------------------------------------------
  - Maximum file size is strictly limited to 12,288 bytes (3 direct blocks).
  - File names cannot exceed 58 characters.
  - Subdirectories, file deletion, and file renaming are not supported 
    as per the project specification.
  - No known bugs or memory/file-handle leaks; passes all validation tests.
======================================================================
