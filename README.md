#include <iostream>
#include <string>
#include <bitset>


std::string padText(const std::string& text) {
    std::string paddedText = text;
    while (paddedText.length() < 128) {
        paddedText += ' ';
    }
    return paddedText;
}


std::string encryptText(const std::string& text) {
    std::string encryptedText;

    for (char c : text) {
 
        int asciiCode = static_cast<int>(c);
        // Отримуємо позицію букви у рядку
        int position = text.find(c);

      
        std::bitset<16> byte;
        byte[0] = (asciiCode % 2 == 0) ? 0 : 1; // Біт парності
        byte |= (asciiCode & 0xFF); // ASCII-код (8 біт)
        byte |= ((position & 0x7F) << 8); // Позиція (7 біт)


        encryptedText += static_cast<char>(byte.to_ulong() >> 8);
        encryptedText += static_cast<char>(byte.to_ulong() & 0xFF);
    }

    return encryptedText;
}

int main() {
    std::string inputText;
    std::cout << "Введіть текст (до 128 символів): ";
    std::getline(std::cin, inputText);

    std::string paddedText = padText(inputText);
    std::string encryptedText = encryptText(paddedText);

    std::cout << "Зашифрований текст: " << encryptedText << std::endl;

    return 0;
}
