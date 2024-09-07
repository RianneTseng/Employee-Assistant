# Employee-Assistant

## Project Description
This is an employee assistant project developed using Langflow and Gradio to help company employees handle meeting information. The assistant can:
- Upload audio files (.wav) to input audio, which is then transcribed into text.
- Use the generated transcript to summarize meeting information.
- Translate the meeting transcript into different languages for others to view.

## Key Features
- **Speech-to-Text**: Upload .wav audio files for real-time speech input. The system will convert the audio into a text transcript using Gradio and OpenAI's Whisper model.
- **Summarization**: Generate meeting summaries based on the transcript.
- **Translation**: Translate the transcript into multiple languages for easy sharing.

## Technologies Used
- **Langflow**: A visual workflow-building tool used to define and manage the project flow.
- **Gradio**: Provides the speech-to-text interface through audio input.
- **OpenAI**: Powers the natural language processing tasks, including summarization and translation.

## How to Use
1. Place the audio file (.wav) in a local directory on your computer.
2. Enter the full file path of the audio file into the text box on the left side of the interface.
3. The system will call Gradio's API to convert the audio file into a text transcript using Whisper model.
4. Once the transcript is generated, it can be used to:
   - Create a meeting summary.
   - Translate the transcript into other languages.

## Demo
### Flow Diagram
Here is the visual representation of the flow used in this project, built with Langflow:
![image](https://github.com/RianneTseng/Employee-Assistant/blob/main/Flow%20Diagram.png)

### Functionality Demo
#### 1. Speech-to-Text and Summarization
![image](https://github.com/RianneTseng/Employee-Assistant/blob/main/Speech2Text%26Summarization.png)

#### 2. Translation
![image](https://github.com/RianneTseng/Employee-Assistant/blob/main/Translation.png)


## Custom Component in Langflow
A custom component has been created in Langflow to handle the interaction with Gradio's API for converting audio to text. Below is the code for the custom component:

```python
from langflow.custom import Component
from langflow.io import MessageTextInput, Output
from langflow.schema import Data
from gradio_client import Client, handle_file


class CustomComponent(Component):
    display_name = "Custom Component"
    description = "Use as a template to create your own component."
    documentation: str = "http://docs.langflow.org/components/custom"
    icon = "custom_components"
    name = "CustomComponent"

    inputs = [
        MessageTextInput(name="input_value", display_name="Input Value", value="Hello, World!"),
    ]

    outputs = [
        Output(display_name="Output", name="output", method="build_output"),
    ]

    def build_output(self) -> Data:
        data = Data(value=self.input_value)

        client = Client("abidlabs/whisper")

        result = client.predict(
            audio=handle_file(self.input_value)
        )

        data = Data(value=result)

        self.status = data
        return data
```

### How the Custom Component Works
- Input: The custom component accepts an audio file path as input.
- Output: It sends the audio file to Gradio's API (abidlabs/whisper) to get the transcription result.
- Integration: The custom component is seamlessly integrated into the Langflow workflow to handle speech-to-text tasks.
    
## Project Files
- `employee assistant.json`: A flow file exported from Langflow.
- `README.md`: Documentation and project notes.
