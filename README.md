# LLM-ASCII_Image

## 간단 설명
Text 2 Text 모델이 이미지를 간접적으로 인식하는 방법에 대한 생각에서 출발하여 증명한 예제입니다.

## LLM-ASCII_Image 의 원리
Text to Text 모델은 Text 를 제외한 Image 나 Audio 를 입력 받지 못합니다.
하지만 전 Text 모델이 Text 를 제외한 Image 와 Audio 를 인식 할 수 있는 모델을 원했습니다.
생각을 해보면 Text 와 Image 는 결국 사람이 각 특징점을 뇌속에서 빠르게 확인하여 각 이미지와 텍스트를 인식하고 구분하여 생각 하게 됩니다.
이에 맞춰 생각해보면 LLM 도 결국 주어진 텍스트를 벡터화 하여 특징점을 찾는 것 입니다.
그렇기에 매우 많은 데이터셋을 가지고 LLM 을 Fine-Tuning 을 하면 어떻게든 특징점을 찾게 되고 그로 인해 AI 는 Image 와 Audio 를 Text 로 변환된 것에 대하여 특징점을 찾게 되는 것입니다.

## Fine-Tuning 데이터셋 제작
제가 제작한 LLM 은 Audio 가 아닌 Image 만을 Fine-Tuning 합니다. Image 중에서도 MNIST 숫자 데이터 셋을 Text 로 변환하여 데이터 셋을 제작 하였습니다.
ASCII 문자인 "▓█ " 이 문자 3개를 사용하여 Image 를 제작 할 것 입니다. Image 를 흑백으로 변환 후 밝기를 3 단계로 나누어 각 픽셀에 대입하였습니다.
그렇게 하여 제작 하면 아래와 같은 데이터 셋이 수백개가 생성 됩니다.
```ASCII_Image
<row:1>                    </row:1>
<row:2>                    </row:2>
<row:3>       ▓▓▓███▓▓█▓   </row:3>
<row:4>      ████▓▓▓       </row:4>
<row:5>        ▓█          </row:5>
<row:6>         ▓██        </row:6>
<row:7>           ▓██      </row:7>
<row:8>         ▓▓███      </row:8>
<row:9>      ▓████▓        </row:9>
<row:10>   ▓▓██▓▓           </row:10>
<row:11>                    </row:11>
```
위의 Text 를 간단히 해석을 하자면 오른쪽과 왼쪽 끝에 <row:N>...</row:N> 과 같은 형태로 데이터가 생성되어 있습니다.
<row:N> 은 행의 크기를 나타 냅니다. <row:N> 은 LLM 에게 숫자를 인식해야 한다는 특징점과 함께 안의 ASCII 데이터를 더욱 잘 케치 할 수 있도록 해줍니다.
데이터 셋은 json 파일에 저장되며, 요소는 다음과 같습니다. 
**Response** 는 LLM 이 ASCII_Image 를 인식하고 응답해야할 정답 입니다.
**ASCII_Image** 는 LLM 이 특징점을 찾게 될 이미지의 ASCII 변환 Text 입니다.
**size** 는 변환된 이미지의 픽셀 크기 입니다.
json 파일로 제작시 다음과 같은 형태가 됩니다.
```json
[
  {
    "size": "20",
    "ASCII_image": "<row:1>                    </row:1>\n<row:2>                    </row:2>\n<row:3>       ▓▓▓███▓▓█▓   </row:3>\n<row:4>      ████▓▓▓       </row:4>\n<row:5>        ▓█          </row:5>\n<row:6>         ▓██        </row:6>\n<row:7>           ▓██      </row:7>\n<row:8>         ▓▓███      </row:8>\n<row:9>      ▓████▓        </row:9>\n<row:10>   ▓▓██▓▓           </row:10>\n<row:11>                    </row:11>\n",
    "Response": "5"
  },
]
```
이렇게 만들어진 데이터 집합체는 다음과 같습니다. => [Synapse-ASCII_Image-Dataset](https://huggingface.co/datasets/RstoneCommand/Synapse-ASCII_image_v01/blob/main/Synapse-ASCII_image_v02.json)

 
