# Instance, Device, Queue


vulkan instance는 애플리케이션의 단위

Device는 하나의 하드웨어나 연결된 하드웨어의 집합

(physical) device는 물리장치
logical device는 instance가 생성하며 device를 감싼다.
logical device는 특정 물리장치와 연결된 자원의 예약을 표현

하나의 물리 장치에서 다양한 logical device가 생성이 가능..


app - interface - physical D - Logical D - Queue

```cpp
void createInstance() {
        if (enableValidationLayers && !checkValidationLayerSupport()) {
            throw std::runtime_error("validation layers requested, but not available!");
        }
        VkApplicationInfo appInfo{};
        appInfo.sType = VK_STRUCTURE_TYPE_APPLICATION_INFO; // stype이 모든 구조체의 첫 멤버
        appInfo.pApplicationName = "Ray Traced Scene";
        appInfo.applicationVersion = VK_MAKE_VERSION(1, 0, 0);
        appInfo.pEngineName = "No Engine";
        appInfo.engineVersion = VK_MAKE_VERSION(1, 0, 0);
        appInfo.apiVersion = VK_API_VERSION_1_2; //사용하는 Vulkan 버전


        VkInstanceCreateInfo createInfo{};
        createInfo.sType = VK_STRUCTURE_TYPE_INSTANCE_CREATE_INFO;
        createInfo.pApplicationInfo = &appInfo;
        auto extensions = getRequiredExtensions();
        createInfo.enabledExtensionCount = static_cast<uint32_t>(extensions.size()); 
        createInfo.ppEnabledExtensionNames = extensions.data();

        VkDebugUtilsMessengerCreateInfoEXT debugCreateInfo{};
        if (enableValidationLayers) {
            createInfo.enabledLayerCount = static_cast<uint32_t>(validationLayers.size());//InstanceLayer의 수
            //layer는 불칸 API 사이에서 로깅, 프로파일링, 디버깅, 추가적인 기능에 이용됨.
            createInfo.ppEnabledLayerNames = validationLayers.data();//instanceLayer 이름
            populateDebugMessengerCreateInfo(debugCreateInfo);
            createInfo.pNext = &debugCreateInfo;
        }
        else {
            createInfo.enabledLayerCount = 0;
            createInfo.pNext = nullptr;
        }

        if (vkCreateInstance(&createInfo, nullptr, &instance) != VK_SUCCESS) {
            throw std::runtime_error("failed to create instance!");
        }

}
```
