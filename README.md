# Hugging_Workflow
Workflow ComfyUi - Create a Hugging and Kissing Video from a Photo
This model is a workflow that allows 2 people to hug when their pictures are given.


The changes made are shown in the video replacing the image cropper nodes and also the errors received are fixed in the code as follows:
2025-03-28T08:24:10.310732 - Traceback (most recent call last):

File "/ComfyUI/execution.py", line 327, in execute

output_data, output_ui, has_subgraph = get_output_data(obj, input_data_all, execution_block_cb=execution_block_cb, pre_execute_cb=pre_execute_cb)

File "/ComfyUI/execution.py", line 202, in get_output_data

return_values = mapnode_over_list(obj, input_data_all, obj.FUNCTION, allow_interrupt=True, execution_block_cb=execution_block_cb, pre_execute_cb=pre_execute_cb)

File "/ComfyUI/execution.py", line 174, in mapnode_over_list

process_inputs(input_dict, i)

File "/ComfyUI/execution.py", line 163, in process_inputs

results.append(getattr(obj, func)(**inputs))

File "/ComfyUI/custom_nodes/comfyui_ttp_toolset/TTP_toolsets.py", line 704, in expand_and_mask

fill_rgba = self.hex_to_rgba(fill_color)

File "/ComfyUI/custom_nodes/comfyui_ttp_toolset/TTP_toolsets.py", line 559, in hex_to_rgba

raise ValueError("Invalid hex color format")

ValueError: Invalid hex color format

2025-03-28T08:24:10.310997 - Prompt executed in 8.00 seconds

Solution to this error :

Check where the expand_and_mask function is called:
Define fill_color variable on line 704 of TTP_toolsets.py.

Verify if it is an invalid HEX code:

HEX color codes should usually have the format #RRGGBB or #RRGGBBAA (example: #FF5733 or #FF5733FF).

If fill_color is in a wrong format (#FF573, 123456, red, etc.), this error will be received.

Validate HEX Format in hex_to_rgba Function

Update the hex_to_rgba function on line 559 of TTP_toolsets.py:

For example, you could add a regex check like the one below:

Change the code to:

import re
def hex_to_rgba(self, hex_color):

# If '#' is present, clear from the beginning

hex_color = hex_color.lstrip('#')


# Check if HEX format is valid

if not re.match(r'^[A-Fa-f0-9]{6}$|^[A-Fa-f0-9]{8}$', hex_color):

raise ValueError(f “Invalid hex color format: {hex_color}. Expected format is #RRRGGBB or #RRRGGBBAA.")

# If 6 characters, return with default alpha value 255 (full opacity)

if len(hex_color) == 6:

r = int(hex_color[0:2], 16)

g = int(hex_color[2:4], 16)

b = int(hex_color[4:6], 16)

return (r, g, b, 255)

# If 8 characters, the last two characters indicate the alpha (transparency) value

elif len(hex_color) == 8:

r = int(hex_color[0:2], 16)

g = int(hex_color[2:4], 16)

b = int(hex_color[4:6], 16)

a = int(hex_color[6:8], 16)

return (r, g, b, a)

else:

raise ValueError(f “Invalid hex color format: {hex_color}”)
