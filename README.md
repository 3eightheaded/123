```markdown
```csharp
using OpenQA.Selenium;
using OpenQA.Selenium.Chrome;
using System;
using NUnit.Framework;

namespace FooterTest
{
    [TestFixture]
    public class FooterTests
    {
        private IWebDriver driver;

        [SetUp]
        public void Setup()
        {   
            driver = new ChromeDriver();
            driver.Manage().Window.Maximize();
        }

        [Test]
        public void TestFooterPresenceAndElements()
        {
            driver.Navigate().GoToUrl("https://only.digital/");

            // Проверка наличия футера
            IWebElement footer = null;
            try
            {
                footer = driver.FindElement(By.TagName("footer"));
            }
            catch (NoSuchElementException)
            {
                Assert.Fail("Футер не найден на странице.");
            }

            try
            {
                var logo = footer.FindElement(By.CssSelector("img[alt='Only']"));
                Assert.IsNotNull(logo, "Логотип компании не найден в футере.");

                var copyrightText = footer.FindElement(By.XPath("//div[contains(text(), '2024-2025')]"));
                Assert.IsNotNull(copyrightText, "Копирайт не найден в футере.");

                var creativeDigitalProductionText = footer.FindElement(By.XPath("//div[contains(text(), 'creative') and contains(text(), 'digital') and contains(text(), 'production')]"));
                Assert.IsNotNull(creativeDigitalProductionText, " 'Creative digital production' не найден в футере.");

                // Проверка ссылок на платформы
                var behanceLink = footer.FindElement(By.CssSelector("a[href='https://www.behance.net/onlydigitalagency']"));
                Assert.IsNotNull(behanceLink, "Ссылка на Behance не найдена в футере.");

                var dprofileLink = footer.FindElement(By.CssSelector("a[href='https://dprofile.ru/only?utm_source=only.digital&utm_medium=referral&utm_campaign=only.digital&utm_referrer=only.digital']"));
                Assert.IsNotNull(dprofileLink, "Ссылка на Dprofile не найдена в футере.");

                var telegramLink = footer.FindElement(By.CssSelector("a[href='https://t.me/onlycreativedigitalagency']"));
                Assert.IsNotNull(telegramLink, "Ссылка на Telegram не найдена в футере.");

                var vkLink = footer.FindElement(By.CssSelector("a[href='https://vk.com/onlydigitalagency']"));
                Assert.IsNotNull(vkLink, "Ссылка на VK не найдена в футере.");

                // Проверка кликабельного телеграм-аккаунта
                var telegramAccount = footer.FindElement(By.XPath("//a[contains(text(), '@onlydigitalagency')]"));
                Assert.IsNotNull(telegramAccount, " Телеграм-аккаунт не найден в футере.");

                var phoneNumber = footer.FindElement(By.XPath("//a[contains(text(), '+7 (495) 740 99 79')]"));
                Assert.IsNotNull(phoneNumber, "Номер телефона не найден в футере.");

                //  PDF
                var pdfButton = footer.FindElement(By.XPath("//a[contains(@href, '.pdf')]"));
                Assert.IsNotNull(pdfButton, "Кнопка для скачивания PDF презентации не найдена в футере.");

                // Pitch
                var pitchButton = footer.FindElement(By.CssSelector("a[href='https://pitch.com/v/only-x9f8ka']"));
                Assert.IsNotNull(pitchButton, "Ссылка на Pitch не найдена в футере.");
            }
            catch (NoSuchElementException ex)
            {
                Assert.Fail($"Элемент не найден в футере: {ex.Message}");
            }
        }

        [TearDown]
        public void TearDown()
        {
            driver.Quit();
        }
    }
}
