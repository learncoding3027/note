import os
import shutil
import time
from pathlib import Path
def copy_files(source_path, destination_path, file_extensions_tuple, file_size_limit_mb, zip_dest_folder = True, delete_og_copied_files=False):
    """
This function copies all the specified type(extension) of files within the specified size limit(in MB) from the source path to the destination path.
It maintains the Folder tree.
"""
    timestr            = time.strftime("%Y%m%d")
    print ('Date of script run: ',timestr)
    print()    
    file_size_limit_mb = file_size_limit_mb ## files GT this size will not be copied
    source_folder      = source_path
    destination_folder = destination_path+'_'+timestr
    ### Define all the required extensions
    extensions         = file_extensions_tuple
    for root, dirs, files in os.walk(source_path):
        for file in files:
            if file.endswith(extensions):
                file_size_mb              = os.path.getsize(root+'\\'+file)/(1024*1024)
                if file_size_mb           <= file_size_limit_mb:
                    source_file_path      = os.path.join(root, file)
                    relative_path         = os.path.relpath(source_file_path, source_folder)
                    destination_file_path = os.path.join(destination_folder, relative_path)
                    os.makedirs(os.path.dirname(destination_file_path), exist_ok=True)
                    shutil.copy(source_file_path, destination_file_path)

    ### Creating the ZIP file of copied paths
    if zip_dest_folder == True:
        shutil.make_archive(destination_folder,'zip', destination_folder)
        print("Zipping the copied files as, ", destination_folder+'.zip')

        if delete_og_copied_files == True:
            ### Deleting the copied files - only keeping the zipped ones
            shutil.rmtree(destination_folder)
            print("Deleted the copied folder - without zipped, ", destination_folder)
    
    return print('\n','\n','Successfully copied the files')
            
            
################################################ Calling the function ################################################            
source_path = Path(r"C:\Users\monish.lalani\OneDrive - Broadcast Audience Research Council\Desktop\NEW\Methodology")
destination_path =Path( r"C:\Users\monish.lalani\OneDrive - Broadcast Audience Research Council\Documents\Methodology_copy")

copy_files(source_path, destination_path, file_extensions_tuple = ('.ipynb', '.csv', '.xlsx', '.py', '.parquet'), file_size_limit_mb = 10, zip_dest_folder = True, delete_og_copied_files=True)


Date of script run:  20231026

---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
Cell In[94], line 46
     43 source_path = Path(r"C:\Users\monish.lalani\OneDrive - Broadcast Audience Research Council\Desktop\NEW\Methodology")
     44 destination_path =Path( r"C:\Users\monish.lalani\OneDrive - Broadcast Audience Research Council\Documents\Methodology_copy")
---> 46 copy_files(source_path, destination_path, file_extensions_tuple = ('.ipynb', '.csv', '.xlsx', '.py', '.parquet'), file_size_limit_mb = 10, zip_dest_folder = True, delete_og_copied_files=True)

Cell In[94], line 15, in copy_files(source_path, destination_path, file_extensions_tuple, file_size_limit_mb, zip_dest_folder, delete_og_copied_files)
     13 file_size_limit_mb = file_size_limit_mb ## files GT this size will not be copied
     14 source_folder      = source_path
---> 15 destination_folder = destination_path+'_'+timestr
     16 ### Define all the required extensions
     17 extensions         = file_extensions_tuple

TypeError: unsupported operand type(s) for +: 'WindowsPath' and 'str'





import os
import shutil
import time
from pathlib import Path

def copy_files(source_path, destination_path, file_extensions_tuple, file_size_limit_mb, zip_dest_folder=True, delete_og_copied_files=False):
    """
    This function copies all the specified type(extension) of files within the specified size limit(in MB) from the source path to the destination path.
    It maintains the Folder tree.
    """
    timestr = time.strftime("%Y%m%d")
    print('Date of script run: ', timestr)
    print()
    file_size_limit_mb = file_size_limit_mb  # files GT this size will not be copied
    source_folder = source_path
    destination_folder = str(destination_path) + '_' + timestr  # Convert destination_path to string
    ### Define all the required extensions
    extensions = file_extensions_tuple
    for root, dirs, files in os.walk(source_path):
        for file in files:
            if file.endswith(extensions):
                file_size_mb = os.path.getsize(root + '\\' + file) / (1024 * 1024)
                if file_size_mb <= file_size_limit_mb:
                    source_file_path = os.path.join(root, file)
                    relative_path = os.path.relpath(source_file_path, source_folder)
                    destination_file_path = os.path.join(destination_folder, relative_path)
                    os.makedirs(os.path.dirname(destination_file_path), exist_ok=True)
                    shutil.copy(source_file_path, destination_file_path)

    ### Creating the ZIP file of copied paths
    if zip_dest_folder == True:
        shutil.make_archive(destination_folder, 'zip', destination_folder)
        print("Zipping the copied files as, ", destination_folder + '.zip')

        if delete_og_copied_files == True:
            ### Deleting the copied files - only keeping the zipped ones
            shutil.rmtree(destination_folder)
            print("Deleted the copied folder - without zipped, ", destination_folder)

    return print('\n', '\n', 'Successfully copied the files')


################################################ Calling the function ################################################
source_path = Path(r"C:\Users\monish.lalani\OneDrive - Broadcast Audience Research Council\Desktop\NEW\Methodology")
destination_path = Path(r"C:\Users\monish.lalani\OneDrive - Broadcast Audience Research Council\Documents\Methodology_copy")

copy_files(source_path, destination_path, file_extensions_tuple=('.ipynb', '.csv', '.xlsx', '.py', '.parquet'),
           file_size_limit_mb=10, zip_dest_folder=True, delete_og_copied_files=True)
           



FileNotFoundError: [Errno 2] No such file or directory: 'C:\\Users\\monish.lalani\\OneDrive - Broadcast Audience Research Council\\Documents\\Methodology_copy_20231026\\eeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee\\gggggggggggggggggggg\\aaaaaaaaaaaaaaaaaaaaaaaa\\bbbbbbbbbbbbbbbbbbbbb\\b2\\Param_Cell_Merging_4RIM_Vijay_MP_with_india_mean-OG.ipynb'

