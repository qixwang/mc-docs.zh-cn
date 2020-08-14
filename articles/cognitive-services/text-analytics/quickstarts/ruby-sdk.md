---
title: 快速入门：适用于 Ruby 的文本分析客户端库
titleSuffix: Azure Cognitive Services
description: 在本快速入门中，使用 Azure 认知服务的 Ruby 文本分析客户端库来检测语言。
services: cognitive-services
author: Johnnytechn
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
origin.date: 05/08/2019
ms.date: 08/03/2020
ms.author: v-johya
ms.openlocfilehash: ec9f6010ee0df2efb0e51db6afbcbfa646049ade
ms.sourcegitcommit: caa18677adb51b5321ad32ae62afcf92ac00b40b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/08/2020
ms.locfileid: "88023402"
---
# <a name="quickstart-use-the-text-analytics-client-library-for-ruby"></a>快速入门：使用适用于 Ruby 的文本分析客户端库

从文本分析客户端库开始操作。 请按照以下步骤安装程序包并试用基本任务的示例代码。

使用文本分析客户端库执行：

* 情绪分析
* 语言检测
* 实体识别
* 关键短语提取

> [!NOTE]
> 本快速入门仅适用于文本分析 2.1 版。 目前，适用于 Ruby 的 v3 客户端库不可用。

[库源代码](https://github.com/Azure/azure-sdk-for-ruby/tree/master/data/azure_cognitiveservices_textanalytics) | [包 (RubyGems)](https://rubygems.org/gems/azure_cognitiveservices_textanalytics) | [示例](https://github.com/Azure-Samples/cognitive-services-quickstart-code)

<a name="HOLTop"></a>

## <a name="prerequisites"></a>先决条件

* Azure 订阅 - [创建试用订阅](https://www.azure.cn/pricing/details/cognitive-services/)
* 最新版本的 [Ruby](https://www.ruby-lang.org/)
* 你有了 Azure 订阅后，<a href="https://portal.azure.cn/#create/Microsoft.CognitiveServicesTextAnalytics"  title="创建文本分析资源"  target="_blank">将在 Azure 门户中创建文本分析资源 <span class="docon docon-navigate-external x-hidden-focus"></span></a>，以获取你的密钥和终结点。 
    * 你需要从创建的资源获取密钥和终结点，以便将应用程序连接到文本分析 API。 稍后会在本快速入门中执行此操作。
    * 可以使用免费定价层试用该服务，然后再升级到付费层进行生产。

## <a name="setting-up"></a>设置

### <a name="create-a-new-ruby-application"></a>创建新的 Ruby 应用程序

在控制台窗口（例如 cmd、PowerShell 或 Bash）中，为应用创建一个新目录并导航到该目录。 然后，创建名为 `GemFile` 的文件，并为你的代码创建一个 Ruby 文件。

```console
mkdir myapp && cd myapp
```

在 `GemFile` 中，添加以下行以将客户端库添加为依赖项。

```ruby
source 'https://rubygems.org'
gem 'azure_cognitiveservices_textanalytics', '~>0.17.3'
```

在 Ruby 文件中，导入以下包。

```ruby
# <includeStatement>
require 'azure_cognitiveservices_textanalytics'
include Azure::CognitiveServices::TextAnalytics::V2_1::Models
# </includeStatement>

class TextAnalyticsClient
  @textAnalyticsClient
  # <initialize> 
  def initialize(endpoint, key)
    credentials =
        MsRestAzure::CognitiveServicesCredentials.new(key)

    endpoint = String.new(endpoint)

    @textAnalyticsClient = Azure::TextAnalytics::Profiles::Latest::Client.new({
        credentials: credentials
    })
    @textAnalyticsClient.endpoint = endpoint
  end
  # </initialize>
  # <analyzeSentiment>
  def AnalyzeSentiment(inputDocuments)
    result = @textAnalyticsClient.sentiment(
        multi_language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== SENTIMENT ANALYSIS ====='
      result.documents.each do |document|
        puts "Document Id: #{document.id}: Sentiment Score: #{document.score}"
      end
    end
    puts ''
  end
  # </analyzeSentiment>
  # <detectLanguage>
  def DetectLanguage(inputDocuments)
    result = @textAnalyticsClient.detect_language(
        language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== LANGUAGE DETECTION ====='
      result.documents.each do |document|
        puts "Document ID: #{document.id} , Language: #{document.detected_languages[0].name}"
      end
    else
      puts 'No results data..'
    end
    puts ''
  end
  # </detectLanguage>
  # <recognizeEntities>
  def RecognizeEntities(inputDocuments)
    result = @textAnalyticsClient.entities(
        multi_language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== ENTITY RECOGNITION ====='
      result.documents.each do |document|
        puts "Document ID: #{document.id}"
          document.entities.each do |entity|
            puts "\tName: #{entity.name}, \tType: #{entity.type == nil ? "N/A": entity.type},\tSub-Type: #{entity.sub_type == nil ? "N/A": entity.sub_type}"
            entity.matches.each do |match|
              puts "\tOffset: #{match.offset}, \Length: #{match.length},\tScore: #{match.entity_type_score}"
            end
            puts
          end
      end
    else
      puts 'No results data..'
    end
    puts ''
  end
  # </recognizeEntities>
  
  # <extractKeyPhrases>
  def ExtractKeyPhrases(inputDocuments)
    result = @textAnalyticsClient.key_phrases(
        multi_language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== KEY PHRASE EXTRACTION ====='
      result.documents.each do |document|
        puts "Document Id: #{document.id}"
        puts '  Key Phrases'
        document.key_phrases.each do |key_phrase|
          puts "    #{key_phrase}"
        end
      end
    else
      puts 'No results data..'
    end
    puts ''
  end
  # </extractKeyPhrases>
end

# <vars>
key_var = "TEXT_ANALYTICS_SUBSCRIPTION_KEY"
if (!ENV[key_var])
    raise "Please set/export the following environment variable: " + key_var
else
    subscription_key = ENV[key_var]
end

endpoint_var = "TEXT_ANALYTICS_ENDPOINT"
if (!ENV[endpoint_var])
    raise "Please set/export the following environment variable: " + endpoint_var
else
    endpoint = ENV[endpoint_var]
end
# </vars>

# <clientCreation>
client = TextAnalyticsClient.new(endpoint, subscription_key)
# </clientCreation>

# <sentimentCall>
def SentimentAnalysisExample(client)
  # The documents to be analyzed. Add the language of the document. The ID can be any value.
  input_1 = MultiLanguageInput.new
  input_1.id = '1'
  input_1.language = 'en'
  input_1.text = 'I had the best day of my life.'

  input_2 = MultiLanguageInput.new
  input_2.id = '2'
  input_2.language = 'en'
  input_2.text = 'This was a waste of my time. The speaker put me to sleep.'

  input_3 = MultiLanguageInput.new
  input_3.id = '3'
  input_3.language = 'es'
  input_3.text = 'No tengo dinero ni nada que dar...'

  input_4 = MultiLanguageInput.new
  input_4.id = '4'
  input_4.language = 'it'
  input_4.text = "L'hotel veneziano era meraviglioso. È un bellissimo pezzo di architettura."

  inputDocuments =  MultiLanguageBatchInput.new
  inputDocuments.documents = [input_1, input_2, input_3, input_4]

  client.AnalyzeSentiment(inputDocuments)
end
# </sentimentCall>
# <detectLanguageCall>
def DetectLanguageExample(client)
 # The documents to be analyzed.
 language_input_1 = LanguageInput.new
 language_input_1.id = '1'
 language_input_1.text = 'This is a document written in English.'

 language_input_2 = LanguageInput.new
 language_input_2.id = '2'
 language_input_2.text = 'Este es un document escrito en Español..'

 language_input_3 = LanguageInput.new
 language_input_3.id = '3'
 language_input_3.text = '这是一个用中文写的文件'

 language_batch_input = LanguageBatchInput.new
 language_batch_input.documents = [language_input_1, language_input_2, language_input_3]

 client.DetectLanguage(language_batch_input)
end
# </detectLanguageCall>
# <recognizeEntitiesCall>
def RecognizeEntitiesExample(client)
  # The documents to be analyzed.
  input_1 = MultiLanguageInput.new
  input_1.id = '1'
  input_1.language = 'en'
  input_1.text = 'Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800.'

  input_2 = MultiLanguageInput.new
  input_2.id = '2'
  input_2.language = 'es'
  input_2.text = 'La sede principal de Microsoft se encuentra en la ciudad de Redmond, a 21 kilómetros de Seattle.'

  multi_language_batch_input =  MultiLanguageBatchInput.new
  multi_language_batch_input.documents = [input_1, input_2]

  client.RecognizeEntities(multi_language_batch_input)
end
# </recognizeEntitiesCall>

# <keyPhrasesCall>
def KeyPhraseExtractionExample(client)
  # The documents to be analyzed.
  input_1 = MultiLanguageInput.new
  input_1.id = '1'
  input_1.language = 'ja'
  input_1.text = '猫は幸せ'

  input_2 = MultiLanguageInput.new
  input_2.id = '2'
  input_2.language = 'de'
  input_2.text = 'Fahrt nach Stuttgart und dann zum Hotel zu Fu.'

  input_3 = MultiLanguageInput.new
  input_3.id = '3'
  input_3.language = 'en'
  input_3.text = 'My cat is stiff as a rock.'

  input_4 = MultiLanguageInput.new
  input_4.id = '4'
  input_4.language = 'es'
  input_4.text = 'A mi me encanta el fútbol!'

  input_documents =  MultiLanguageBatchInput.new
  input_documents.documents = [input_1, input_2, input_3, input_4]

  client.ExtractKeyPhrases(input_documents)
end
# </keyPhrasesCall>
DetectLanguageExample(client)
SentimentAnalysisExample(client)
RecognizeEntitiesExample(client)
KeyPhraseExtractionExample(client)
```

为资源的 Azure 终结点和密钥创建变量。 

[!INCLUDE [text-analytics-find-resource-information](../includes/find-azure-resource-info.md)]

```ruby
const subscription_key = '<paste-your-text-analytics-key-here>'
const endpoint = `<paste-your-text-analytics-endpoint-here>`
```

## <a name="object-model"></a>对象模型 

文本分析客户端使用你的密钥向 Azure 进行身份验证。 该客户端提供了几种方法来分析文本，文本可以是单个字符串，也可以是批处理。 

文本将以 `documents` 的列表的形式发送到 API，该项是包含 `id`、`text` 和 `language` 属性的组合的 `dictionary` 对象，具体取决于所用的方法。 `text` 属性存储要以源 `language` 分析的文本，而 `id` 则可以是任何值。 

响应对象是一个列表，其中包含每个文档的分析信息。 

## <a name="code-examples"></a>代码示例

这些代码片段演示如何使用适用于 Ruby 的文本分析客户端库执行以下操作：

* [对客户端进行身份验证](#authenticate-the-client)
* [情绪分析](#sentiment-analysis)
* [语言检测](#language-detection)
* [实体识别](#entity-recognition)
* [关键短语提取](#key-phrase-extraction)

## <a name="authenticate-the-client"></a>验证客户端

创建名为的 `TextAnalyticsClient` 的类。 

```ruby
class TextAnalyticsClient
  @textAnalyticsClient
  #...
end
```

在此类中，创建名为 `initialize` 的函数以使用密钥和终结点对客户端进行身份验证。 

```ruby
# <includeStatement>
require 'azure_cognitiveservices_textanalytics'
include Azure::CognitiveServices::TextAnalytics::V2_1::Models
# </includeStatement>

class TextAnalyticsClient
  @textAnalyticsClient
  # <initialize> 
  def initialize(endpoint, key)
    credentials =
        MsRestAzure::CognitiveServicesCredentials.new(key)

    endpoint = String.new(endpoint)

    @textAnalyticsClient = Azure::TextAnalytics::Profiles::Latest::Client.new({
        credentials: credentials
    })
    @textAnalyticsClient.endpoint = endpoint
  end
  # </initialize>
  # <analyzeSentiment>
  def AnalyzeSentiment(inputDocuments)
    result = @textAnalyticsClient.sentiment(
        multi_language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== SENTIMENT ANALYSIS ====='
      result.documents.each do |document|
        puts "Document Id: #{document.id}: Sentiment Score: #{document.score}"
      end
    end
    puts ''
  end
  # </analyzeSentiment>
  # <detectLanguage>
  def DetectLanguage(inputDocuments)
    result = @textAnalyticsClient.detect_language(
        language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== LANGUAGE DETECTION ====='
      result.documents.each do |document|
        puts "Document ID: #{document.id} , Language: #{document.detected_languages[0].name}"
      end
    else
      puts 'No results data..'
    end
    puts ''
  end
  # </detectLanguage>
  # <recognizeEntities>
  def RecognizeEntities(inputDocuments)
    result = @textAnalyticsClient.entities(
        multi_language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== ENTITY RECOGNITION ====='
      result.documents.each do |document|
        puts "Document ID: #{document.id}"
          document.entities.each do |entity|
            puts "\tName: #{entity.name}, \tType: #{entity.type == nil ? "N/A": entity.type},\tSub-Type: #{entity.sub_type == nil ? "N/A": entity.sub_type}"
            entity.matches.each do |match|
              puts "\tOffset: #{match.offset}, \Length: #{match.length},\tScore: #{match.entity_type_score}"
            end
            puts
          end
      end
    else
      puts 'No results data..'
    end
    puts ''
  end
  # </recognizeEntities>
  
  # <extractKeyPhrases>
  def ExtractKeyPhrases(inputDocuments)
    result = @textAnalyticsClient.key_phrases(
        multi_language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== KEY PHRASE EXTRACTION ====='
      result.documents.each do |document|
        puts "Document Id: #{document.id}"
        puts '  Key Phrases'
        document.key_phrases.each do |key_phrase|
          puts "    #{key_phrase}"
        end
      end
    else
      puts 'No results data..'
    end
    puts ''
  end
  # </extractKeyPhrases>
end

# <vars>
key_var = "TEXT_ANALYTICS_SUBSCRIPTION_KEY"
if (!ENV[key_var])
    raise "Please set/export the following environment variable: " + key_var
else
    subscription_key = ENV[key_var]
end

endpoint_var = "TEXT_ANALYTICS_ENDPOINT"
if (!ENV[endpoint_var])
    raise "Please set/export the following environment variable: " + endpoint_var
else
    endpoint = ENV[endpoint_var]
end
# </vars>

# <clientCreation>
client = TextAnalyticsClient.new(endpoint, subscription_key)
# </clientCreation>

# <sentimentCall>
def SentimentAnalysisExample(client)
  # The documents to be analyzed. Add the language of the document. The ID can be any value.
  input_1 = MultiLanguageInput.new
  input_1.id = '1'
  input_1.language = 'en'
  input_1.text = 'I had the best day of my life.'

  input_2 = MultiLanguageInput.new
  input_2.id = '2'
  input_2.language = 'en'
  input_2.text = 'This was a waste of my time. The speaker put me to sleep.'

  input_3 = MultiLanguageInput.new
  input_3.id = '3'
  input_3.language = 'es'
  input_3.text = 'No tengo dinero ni nada que dar...'

  input_4 = MultiLanguageInput.new
  input_4.id = '4'
  input_4.language = 'it'
  input_4.text = "L'hotel veneziano era meraviglioso. È un bellissimo pezzo di architettura."

  inputDocuments =  MultiLanguageBatchInput.new
  inputDocuments.documents = [input_1, input_2, input_3, input_4]

  client.AnalyzeSentiment(inputDocuments)
end
# </sentimentCall>
# <detectLanguageCall>
def DetectLanguageExample(client)
 # The documents to be analyzed.
 language_input_1 = LanguageInput.new
 language_input_1.id = '1'
 language_input_1.text = 'This is a document written in English.'

 language_input_2 = LanguageInput.new
 language_input_2.id = '2'
 language_input_2.text = 'Este es un document escrito en Español..'

 language_input_3 = LanguageInput.new
 language_input_3.id = '3'
 language_input_3.text = '这是一个用中文写的文件'

 language_batch_input = LanguageBatchInput.new
 language_batch_input.documents = [language_input_1, language_input_2, language_input_3]

 client.DetectLanguage(language_batch_input)
end
# </detectLanguageCall>
# <recognizeEntitiesCall>
def RecognizeEntitiesExample(client)
  # The documents to be analyzed.
  input_1 = MultiLanguageInput.new
  input_1.id = '1'
  input_1.language = 'en'
  input_1.text = 'Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800.'

  input_2 = MultiLanguageInput.new
  input_2.id = '2'
  input_2.language = 'es'
  input_2.text = 'La sede principal de Microsoft se encuentra en la ciudad de Redmond, a 21 kilómetros de Seattle.'

  multi_language_batch_input =  MultiLanguageBatchInput.new
  multi_language_batch_input.documents = [input_1, input_2]

  client.RecognizeEntities(multi_language_batch_input)
end
# </recognizeEntitiesCall>

# <keyPhrasesCall>
def KeyPhraseExtractionExample(client)
  # The documents to be analyzed.
  input_1 = MultiLanguageInput.new
  input_1.id = '1'
  input_1.language = 'ja'
  input_1.text = '猫は幸せ'

  input_2 = MultiLanguageInput.new
  input_2.id = '2'
  input_2.language = 'de'
  input_2.text = 'Fahrt nach Stuttgart und dann zum Hotel zu Fu.'

  input_3 = MultiLanguageInput.new
  input_3.id = '3'
  input_3.language = 'en'
  input_3.text = 'My cat is stiff as a rock.'

  input_4 = MultiLanguageInput.new
  input_4.id = '4'
  input_4.language = 'es'
  input_4.text = 'A mi me encanta el fútbol!'

  input_documents =  MultiLanguageBatchInput.new
  input_documents.documents = [input_1, input_2, input_3, input_4]

  client.ExtractKeyPhrases(input_documents)
end
# </keyPhrasesCall>
DetectLanguageExample(client)
SentimentAnalysisExample(client)
RecognizeEntitiesExample(client)
KeyPhraseExtractionExample(client)
```

在类的外部，使用客户端的 `new()` 函数对其进行实例化。

```ruby
# <includeStatement>
require 'azure_cognitiveservices_textanalytics'
include Azure::CognitiveServices::TextAnalytics::V2_1::Models
# </includeStatement>

class TextAnalyticsClient
  @textAnalyticsClient
  # <initialize> 
  def initialize(endpoint, key)
    credentials =
        MsRestAzure::CognitiveServicesCredentials.new(key)

    endpoint = String.new(endpoint)

    @textAnalyticsClient = Azure::TextAnalytics::Profiles::Latest::Client.new({
        credentials: credentials
    })
    @textAnalyticsClient.endpoint = endpoint
  end
  # </initialize>
  # <analyzeSentiment>
  def AnalyzeSentiment(inputDocuments)
    result = @textAnalyticsClient.sentiment(
        multi_language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== SENTIMENT ANALYSIS ====='
      result.documents.each do |document|
        puts "Document Id: #{document.id}: Sentiment Score: #{document.score}"
      end
    end
    puts ''
  end
  # </analyzeSentiment>
  # <detectLanguage>
  def DetectLanguage(inputDocuments)
    result = @textAnalyticsClient.detect_language(
        language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== LANGUAGE DETECTION ====='
      result.documents.each do |document|
        puts "Document ID: #{document.id} , Language: #{document.detected_languages[0].name}"
      end
    else
      puts 'No results data..'
    end
    puts ''
  end
  # </detectLanguage>
  # <recognizeEntities>
  def RecognizeEntities(inputDocuments)
    result = @textAnalyticsClient.entities(
        multi_language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== ENTITY RECOGNITION ====='
      result.documents.each do |document|
        puts "Document ID: #{document.id}"
          document.entities.each do |entity|
            puts "\tName: #{entity.name}, \tType: #{entity.type == nil ? "N/A": entity.type},\tSub-Type: #{entity.sub_type == nil ? "N/A": entity.sub_type}"
            entity.matches.each do |match|
              puts "\tOffset: #{match.offset}, \Length: #{match.length},\tScore: #{match.entity_type_score}"
            end
            puts
          end
      end
    else
      puts 'No results data..'
    end
    puts ''
  end
  # </recognizeEntities>
  
  # <extractKeyPhrases>
  def ExtractKeyPhrases(inputDocuments)
    result = @textAnalyticsClient.key_phrases(
        multi_language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== KEY PHRASE EXTRACTION ====='
      result.documents.each do |document|
        puts "Document Id: #{document.id}"
        puts '  Key Phrases'
        document.key_phrases.each do |key_phrase|
          puts "    #{key_phrase}"
        end
      end
    else
      puts 'No results data..'
    end
    puts ''
  end
  # </extractKeyPhrases>
end

# <vars>
key_var = "TEXT_ANALYTICS_SUBSCRIPTION_KEY"
if (!ENV[key_var])
    raise "Please set/export the following environment variable: " + key_var
else
    subscription_key = ENV[key_var]
end

endpoint_var = "TEXT_ANALYTICS_ENDPOINT"
if (!ENV[endpoint_var])
    raise "Please set/export the following environment variable: " + endpoint_var
else
    endpoint = ENV[endpoint_var]
end
# </vars>

# <clientCreation>
client = TextAnalyticsClient.new(endpoint, subscription_key)
# </clientCreation>

# <sentimentCall>
def SentimentAnalysisExample(client)
  # The documents to be analyzed. Add the language of the document. The ID can be any value.
  input_1 = MultiLanguageInput.new
  input_1.id = '1'
  input_1.language = 'en'
  input_1.text = 'I had the best day of my life.'

  input_2 = MultiLanguageInput.new
  input_2.id = '2'
  input_2.language = 'en'
  input_2.text = 'This was a waste of my time. The speaker put me to sleep.'

  input_3 = MultiLanguageInput.new
  input_3.id = '3'
  input_3.language = 'es'
  input_3.text = 'No tengo dinero ni nada que dar...'

  input_4 = MultiLanguageInput.new
  input_4.id = '4'
  input_4.language = 'it'
  input_4.text = "L'hotel veneziano era meraviglioso. È un bellissimo pezzo di architettura."

  inputDocuments =  MultiLanguageBatchInput.new
  inputDocuments.documents = [input_1, input_2, input_3, input_4]

  client.AnalyzeSentiment(inputDocuments)
end
# </sentimentCall>
# <detectLanguageCall>
def DetectLanguageExample(client)
 # The documents to be analyzed.
 language_input_1 = LanguageInput.new
 language_input_1.id = '1'
 language_input_1.text = 'This is a document written in English.'

 language_input_2 = LanguageInput.new
 language_input_2.id = '2'
 language_input_2.text = 'Este es un document escrito en Español..'

 language_input_3 = LanguageInput.new
 language_input_3.id = '3'
 language_input_3.text = '这是一个用中文写的文件'

 language_batch_input = LanguageBatchInput.new
 language_batch_input.documents = [language_input_1, language_input_2, language_input_3]

 client.DetectLanguage(language_batch_input)
end
# </detectLanguageCall>
# <recognizeEntitiesCall>
def RecognizeEntitiesExample(client)
  # The documents to be analyzed.
  input_1 = MultiLanguageInput.new
  input_1.id = '1'
  input_1.language = 'en'
  input_1.text = 'Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800.'

  input_2 = MultiLanguageInput.new
  input_2.id = '2'
  input_2.language = 'es'
  input_2.text = 'La sede principal de Microsoft se encuentra en la ciudad de Redmond, a 21 kilómetros de Seattle.'

  multi_language_batch_input =  MultiLanguageBatchInput.new
  multi_language_batch_input.documents = [input_1, input_2]

  client.RecognizeEntities(multi_language_batch_input)
end
# </recognizeEntitiesCall>

# <keyPhrasesCall>
def KeyPhraseExtractionExample(client)
  # The documents to be analyzed.
  input_1 = MultiLanguageInput.new
  input_1.id = '1'
  input_1.language = 'ja'
  input_1.text = '猫は幸せ'

  input_2 = MultiLanguageInput.new
  input_2.id = '2'
  input_2.language = 'de'
  input_2.text = 'Fahrt nach Stuttgart und dann zum Hotel zu Fu.'

  input_3 = MultiLanguageInput.new
  input_3.id = '3'
  input_3.language = 'en'
  input_3.text = 'My cat is stiff as a rock.'

  input_4 = MultiLanguageInput.new
  input_4.id = '4'
  input_4.language = 'es'
  input_4.text = 'A mi me encanta el fútbol!'

  input_documents =  MultiLanguageBatchInput.new
  input_documents.documents = [input_1, input_2, input_3, input_4]

  client.ExtractKeyPhrases(input_documents)
end
# </keyPhrasesCall>
DetectLanguageExample(client)
SentimentAnalysisExample(client)
RecognizeEntitiesExample(client)
KeyPhraseExtractionExample(client)
```

<a name="SentimentAnalysis"></a>

## <a name="sentiment-analysis"></a>情绪分析

在客户端对象中，创建名为 `AnalyzeSentiment()` 的函数，该函数采用稍后将创建的输入文档的列表。 调用客户端的 `sentiment()` 函数并获取结果。 然后循环访问结果，输出每个文档的 ID 和情绪分数。 评分接近 0 表示消极情绪，评分接近 1 表示积极情绪。

```ruby
# <includeStatement>
require 'azure_cognitiveservices_textanalytics'
include Azure::CognitiveServices::TextAnalytics::V2_1::Models
# </includeStatement>

class TextAnalyticsClient
  @textAnalyticsClient
  # <initialize> 
  def initialize(endpoint, key)
    credentials =
        MsRestAzure::CognitiveServicesCredentials.new(key)

    endpoint = String.new(endpoint)

    @textAnalyticsClient = Azure::TextAnalytics::Profiles::Latest::Client.new({
        credentials: credentials
    })
    @textAnalyticsClient.endpoint = endpoint
  end
  # </initialize>
  # <analyzeSentiment>
  def AnalyzeSentiment(inputDocuments)
    result = @textAnalyticsClient.sentiment(
        multi_language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== SENTIMENT ANALYSIS ====='
      result.documents.each do |document|
        puts "Document Id: #{document.id}: Sentiment Score: #{document.score}"
      end
    end
    puts ''
  end
  # </analyzeSentiment>
  # <detectLanguage>
  def DetectLanguage(inputDocuments)
    result = @textAnalyticsClient.detect_language(
        language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== LANGUAGE DETECTION ====='
      result.documents.each do |document|
        puts "Document ID: #{document.id} , Language: #{document.detected_languages[0].name}"
      end
    else
      puts 'No results data..'
    end
    puts ''
  end
  # </detectLanguage>
  # <recognizeEntities>
  def RecognizeEntities(inputDocuments)
    result = @textAnalyticsClient.entities(
        multi_language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== ENTITY RECOGNITION ====='
      result.documents.each do |document|
        puts "Document ID: #{document.id}"
          document.entities.each do |entity|
            puts "\tName: #{entity.name}, \tType: #{entity.type == nil ? "N/A": entity.type},\tSub-Type: #{entity.sub_type == nil ? "N/A": entity.sub_type}"
            entity.matches.each do |match|
              puts "\tOffset: #{match.offset}, \Length: #{match.length},\tScore: #{match.entity_type_score}"
            end
            puts
          end
      end
    else
      puts 'No results data..'
    end
    puts ''
  end
  # </recognizeEntities>
  
  # <extractKeyPhrases>
  def ExtractKeyPhrases(inputDocuments)
    result = @textAnalyticsClient.key_phrases(
        multi_language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== KEY PHRASE EXTRACTION ====='
      result.documents.each do |document|
        puts "Document Id: #{document.id}"
        puts '  Key Phrases'
        document.key_phrases.each do |key_phrase|
          puts "    #{key_phrase}"
        end
      end
    else
      puts 'No results data..'
    end
    puts ''
  end
  # </extractKeyPhrases>
end

# <vars>
key_var = "TEXT_ANALYTICS_SUBSCRIPTION_KEY"
if (!ENV[key_var])
    raise "Please set/export the following environment variable: " + key_var
else
    subscription_key = ENV[key_var]
end

endpoint_var = "TEXT_ANALYTICS_ENDPOINT"
if (!ENV[endpoint_var])
    raise "Please set/export the following environment variable: " + endpoint_var
else
    endpoint = ENV[endpoint_var]
end
# </vars>

# <clientCreation>
client = TextAnalyticsClient.new(endpoint, subscription_key)
# </clientCreation>

# <sentimentCall>
def SentimentAnalysisExample(client)
  # The documents to be analyzed. Add the language of the document. The ID can be any value.
  input_1 = MultiLanguageInput.new
  input_1.id = '1'
  input_1.language = 'en'
  input_1.text = 'I had the best day of my life.'

  input_2 = MultiLanguageInput.new
  input_2.id = '2'
  input_2.language = 'en'
  input_2.text = 'This was a waste of my time. The speaker put me to sleep.'

  input_3 = MultiLanguageInput.new
  input_3.id = '3'
  input_3.language = 'es'
  input_3.text = 'No tengo dinero ni nada que dar...'

  input_4 = MultiLanguageInput.new
  input_4.id = '4'
  input_4.language = 'it'
  input_4.text = "L'hotel veneziano era meraviglioso. È un bellissimo pezzo di architettura."

  inputDocuments =  MultiLanguageBatchInput.new
  inputDocuments.documents = [input_1, input_2, input_3, input_4]

  client.AnalyzeSentiment(inputDocuments)
end
# </sentimentCall>
# <detectLanguageCall>
def DetectLanguageExample(client)
 # The documents to be analyzed.
 language_input_1 = LanguageInput.new
 language_input_1.id = '1'
 language_input_1.text = 'This is a document written in English.'

 language_input_2 = LanguageInput.new
 language_input_2.id = '2'
 language_input_2.text = 'Este es un document escrito en Español..'

 language_input_3 = LanguageInput.new
 language_input_3.id = '3'
 language_input_3.text = '这是一个用中文写的文件'

 language_batch_input = LanguageBatchInput.new
 language_batch_input.documents = [language_input_1, language_input_2, language_input_3]

 client.DetectLanguage(language_batch_input)
end
# </detectLanguageCall>
# <recognizeEntitiesCall>
def RecognizeEntitiesExample(client)
  # The documents to be analyzed.
  input_1 = MultiLanguageInput.new
  input_1.id = '1'
  input_1.language = 'en'
  input_1.text = 'Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800.'

  input_2 = MultiLanguageInput.new
  input_2.id = '2'
  input_2.language = 'es'
  input_2.text = 'La sede principal de Microsoft se encuentra en la ciudad de Redmond, a 21 kilómetros de Seattle.'

  multi_language_batch_input =  MultiLanguageBatchInput.new
  multi_language_batch_input.documents = [input_1, input_2]

  client.RecognizeEntities(multi_language_batch_input)
end
# </recognizeEntitiesCall>

# <keyPhrasesCall>
def KeyPhraseExtractionExample(client)
  # The documents to be analyzed.
  input_1 = MultiLanguageInput.new
  input_1.id = '1'
  input_1.language = 'ja'
  input_1.text = '猫は幸せ'

  input_2 = MultiLanguageInput.new
  input_2.id = '2'
  input_2.language = 'de'
  input_2.text = 'Fahrt nach Stuttgart und dann zum Hotel zu Fu.'

  input_3 = MultiLanguageInput.new
  input_3.id = '3'
  input_3.language = 'en'
  input_3.text = 'My cat is stiff as a rock.'

  input_4 = MultiLanguageInput.new
  input_4.id = '4'
  input_4.language = 'es'
  input_4.text = 'A mi me encanta el fútbol!'

  input_documents =  MultiLanguageBatchInput.new
  input_documents.documents = [input_1, input_2, input_3, input_4]

  client.ExtractKeyPhrases(input_documents)
end
# </keyPhrasesCall>
DetectLanguageExample(client)
SentimentAnalysisExample(client)
RecognizeEntitiesExample(client)
KeyPhraseExtractionExample(client)
```

在客户端函数外部，创建名为 `SentimentAnalysisExample()` 的新函数，该函数使用以前创建的 `TextAnalyticsClient` 对象。 创建 `MultiLanguageInput` 对象的列表，其中包含需分析的文档。 每个对象会包含 `id`、`Language` 和 `text` 属性。 `text` 属性存储要分析的文本，`language` 是文档的语言，`id` 则可以是任何值。 然后调用客户端的 `AnalyzeSentiment()` 函数。

```ruby
# <includeStatement>
require 'azure_cognitiveservices_textanalytics'
include Azure::CognitiveServices::TextAnalytics::V2_1::Models
# </includeStatement>

class TextAnalyticsClient
  @textAnalyticsClient
  # <initialize> 
  def initialize(endpoint, key)
    credentials =
        MsRestAzure::CognitiveServicesCredentials.new(key)

    endpoint = String.new(endpoint)

    @textAnalyticsClient = Azure::TextAnalytics::Profiles::Latest::Client.new({
        credentials: credentials
    })
    @textAnalyticsClient.endpoint = endpoint
  end
  # </initialize>
  # <analyzeSentiment>
  def AnalyzeSentiment(inputDocuments)
    result = @textAnalyticsClient.sentiment(
        multi_language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== SENTIMENT ANALYSIS ====='
      result.documents.each do |document|
        puts "Document Id: #{document.id}: Sentiment Score: #{document.score}"
      end
    end
    puts ''
  end
  # </analyzeSentiment>
  # <detectLanguage>
  def DetectLanguage(inputDocuments)
    result = @textAnalyticsClient.detect_language(
        language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== LANGUAGE DETECTION ====='
      result.documents.each do |document|
        puts "Document ID: #{document.id} , Language: #{document.detected_languages[0].name}"
      end
    else
      puts 'No results data..'
    end
    puts ''
  end
  # </detectLanguage>
  # <recognizeEntities>
  def RecognizeEntities(inputDocuments)
    result = @textAnalyticsClient.entities(
        multi_language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== ENTITY RECOGNITION ====='
      result.documents.each do |document|
        puts "Document ID: #{document.id}"
          document.entities.each do |entity|
            puts "\tName: #{entity.name}, \tType: #{entity.type == nil ? "N/A": entity.type},\tSub-Type: #{entity.sub_type == nil ? "N/A": entity.sub_type}"
            entity.matches.each do |match|
              puts "\tOffset: #{match.offset}, \Length: #{match.length},\tScore: #{match.entity_type_score}"
            end
            puts
          end
      end
    else
      puts 'No results data..'
    end
    puts ''
  end
  # </recognizeEntities>
  
  # <extractKeyPhrases>
  def ExtractKeyPhrases(inputDocuments)
    result = @textAnalyticsClient.key_phrases(
        multi_language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== KEY PHRASE EXTRACTION ====='
      result.documents.each do |document|
        puts "Document Id: #{document.id}"
        puts '  Key Phrases'
        document.key_phrases.each do |key_phrase|
          puts "    #{key_phrase}"
        end
      end
    else
      puts 'No results data..'
    end
    puts ''
  end
  # </extractKeyPhrases>
end

# <vars>
key_var = "TEXT_ANALYTICS_SUBSCRIPTION_KEY"
if (!ENV[key_var])
    raise "Please set/export the following environment variable: " + key_var
else
    subscription_key = ENV[key_var]
end

endpoint_var = "TEXT_ANALYTICS_ENDPOINT"
if (!ENV[endpoint_var])
    raise "Please set/export the following environment variable: " + endpoint_var
else
    endpoint = ENV[endpoint_var]
end
# </vars>

# <clientCreation>
client = TextAnalyticsClient.new(endpoint, subscription_key)
# </clientCreation>

# <sentimentCall>
def SentimentAnalysisExample(client)
  # The documents to be analyzed. Add the language of the document. The ID can be any value.
  input_1 = MultiLanguageInput.new
  input_1.id = '1'
  input_1.language = 'en'
  input_1.text = 'I had the best day of my life.'

  input_2 = MultiLanguageInput.new
  input_2.id = '2'
  input_2.language = 'en'
  input_2.text = 'This was a waste of my time. The speaker put me to sleep.'

  input_3 = MultiLanguageInput.new
  input_3.id = '3'
  input_3.language = 'es'
  input_3.text = 'No tengo dinero ni nada que dar...'

  input_4 = MultiLanguageInput.new
  input_4.id = '4'
  input_4.language = 'it'
  input_4.text = "L'hotel veneziano era meraviglioso. È un bellissimo pezzo di architettura."

  inputDocuments =  MultiLanguageBatchInput.new
  inputDocuments.documents = [input_1, input_2, input_3, input_4]

  client.AnalyzeSentiment(inputDocuments)
end
# </sentimentCall>
# <detectLanguageCall>
def DetectLanguageExample(client)
 # The documents to be analyzed.
 language_input_1 = LanguageInput.new
 language_input_1.id = '1'
 language_input_1.text = 'This is a document written in English.'

 language_input_2 = LanguageInput.new
 language_input_2.id = '2'
 language_input_2.text = 'Este es un document escrito en Español..'

 language_input_3 = LanguageInput.new
 language_input_3.id = '3'
 language_input_3.text = '这是一个用中文写的文件'

 language_batch_input = LanguageBatchInput.new
 language_batch_input.documents = [language_input_1, language_input_2, language_input_3]

 client.DetectLanguage(language_batch_input)
end
# </detectLanguageCall>
# <recognizeEntitiesCall>
def RecognizeEntitiesExample(client)
  # The documents to be analyzed.
  input_1 = MultiLanguageInput.new
  input_1.id = '1'
  input_1.language = 'en'
  input_1.text = 'Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800.'

  input_2 = MultiLanguageInput.new
  input_2.id = '2'
  input_2.language = 'es'
  input_2.text = 'La sede principal de Microsoft se encuentra en la ciudad de Redmond, a 21 kilómetros de Seattle.'

  multi_language_batch_input =  MultiLanguageBatchInput.new
  multi_language_batch_input.documents = [input_1, input_2]

  client.RecognizeEntities(multi_language_batch_input)
end
# </recognizeEntitiesCall>

# <keyPhrasesCall>
def KeyPhraseExtractionExample(client)
  # The documents to be analyzed.
  input_1 = MultiLanguageInput.new
  input_1.id = '1'
  input_1.language = 'ja'
  input_1.text = '猫は幸せ'

  input_2 = MultiLanguageInput.new
  input_2.id = '2'
  input_2.language = 'de'
  input_2.text = 'Fahrt nach Stuttgart und dann zum Hotel zu Fu.'

  input_3 = MultiLanguageInput.new
  input_3.id = '3'
  input_3.language = 'en'
  input_3.text = 'My cat is stiff as a rock.'

  input_4 = MultiLanguageInput.new
  input_4.id = '4'
  input_4.language = 'es'
  input_4.text = 'A mi me encanta el fútbol!'

  input_documents =  MultiLanguageBatchInput.new
  input_documents.documents = [input_1, input_2, input_3, input_4]

  client.ExtractKeyPhrases(input_documents)
end
# </keyPhrasesCall>
DetectLanguageExample(client)
SentimentAnalysisExample(client)
RecognizeEntitiesExample(client)
KeyPhraseExtractionExample(client)
```

调用 `SentimentAnalysisExample()` 函数。

```ruby
SentimentAnalysisExample(textAnalyticsClient)
```

### <a name="output"></a>输出

```console
===== SENTIMENT ANALYSIS =====
Document ID: 1 , Sentiment Score: 0.87
Document ID: 2 , Sentiment Score: 0.11
Document ID: 3 , Sentiment Score: 0.44
Document ID: 4 , Sentiment Score: 1.00
```

<a name="LanguageDetection"></a>

## <a name="language-detection"></a>语言检测

在客户端对象中，创建名为 `DetectLanguage()` 的函数，该函数采用稍后将创建的输入文档的列表。 调用客户端的 `detect_language()` 函数并获取结果。 然后循环访问结果，输出每个文档的 ID 和检测到的语言。

```ruby
# <includeStatement>
require 'azure_cognitiveservices_textanalytics'
include Azure::CognitiveServices::TextAnalytics::V2_1::Models
# </includeStatement>

class TextAnalyticsClient
  @textAnalyticsClient
  # <initialize> 
  def initialize(endpoint, key)
    credentials =
        MsRestAzure::CognitiveServicesCredentials.new(key)

    endpoint = String.new(endpoint)

    @textAnalyticsClient = Azure::TextAnalytics::Profiles::Latest::Client.new({
        credentials: credentials
    })
    @textAnalyticsClient.endpoint = endpoint
  end
  # </initialize>
  # <analyzeSentiment>
  def AnalyzeSentiment(inputDocuments)
    result = @textAnalyticsClient.sentiment(
        multi_language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== SENTIMENT ANALYSIS ====='
      result.documents.each do |document|
        puts "Document Id: #{document.id}: Sentiment Score: #{document.score}"
      end
    end
    puts ''
  end
  # </analyzeSentiment>
  # <detectLanguage>
  def DetectLanguage(inputDocuments)
    result = @textAnalyticsClient.detect_language(
        language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== LANGUAGE DETECTION ====='
      result.documents.each do |document|
        puts "Document ID: #{document.id} , Language: #{document.detected_languages[0].name}"
      end
    else
      puts 'No results data..'
    end
    puts ''
  end
  # </detectLanguage>
  # <recognizeEntities>
  def RecognizeEntities(inputDocuments)
    result = @textAnalyticsClient.entities(
        multi_language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== ENTITY RECOGNITION ====='
      result.documents.each do |document|
        puts "Document ID: #{document.id}"
          document.entities.each do |entity|
            puts "\tName: #{entity.name}, \tType: #{entity.type == nil ? "N/A": entity.type},\tSub-Type: #{entity.sub_type == nil ? "N/A": entity.sub_type}"
            entity.matches.each do |match|
              puts "\tOffset: #{match.offset}, \Length: #{match.length},\tScore: #{match.entity_type_score}"
            end
            puts
          end
      end
    else
      puts 'No results data..'
    end
    puts ''
  end
  # </recognizeEntities>
  
  # <extractKeyPhrases>
  def ExtractKeyPhrases(inputDocuments)
    result = @textAnalyticsClient.key_phrases(
        multi_language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== KEY PHRASE EXTRACTION ====='
      result.documents.each do |document|
        puts "Document Id: #{document.id}"
        puts '  Key Phrases'
        document.key_phrases.each do |key_phrase|
          puts "    #{key_phrase}"
        end
      end
    else
      puts 'No results data..'
    end
    puts ''
  end
  # </extractKeyPhrases>
end

# <vars>
key_var = "TEXT_ANALYTICS_SUBSCRIPTION_KEY"
if (!ENV[key_var])
    raise "Please set/export the following environment variable: " + key_var
else
    subscription_key = ENV[key_var]
end

endpoint_var = "TEXT_ANALYTICS_ENDPOINT"
if (!ENV[endpoint_var])
    raise "Please set/export the following environment variable: " + endpoint_var
else
    endpoint = ENV[endpoint_var]
end
# </vars>

# <clientCreation>
client = TextAnalyticsClient.new(endpoint, subscription_key)
# </clientCreation>

# <sentimentCall>
def SentimentAnalysisExample(client)
  # The documents to be analyzed. Add the language of the document. The ID can be any value.
  input_1 = MultiLanguageInput.new
  input_1.id = '1'
  input_1.language = 'en'
  input_1.text = 'I had the best day of my life.'

  input_2 = MultiLanguageInput.new
  input_2.id = '2'
  input_2.language = 'en'
  input_2.text = 'This was a waste of my time. The speaker put me to sleep.'

  input_3 = MultiLanguageInput.new
  input_3.id = '3'
  input_3.language = 'es'
  input_3.text = 'No tengo dinero ni nada que dar...'

  input_4 = MultiLanguageInput.new
  input_4.id = '4'
  input_4.language = 'it'
  input_4.text = "L'hotel veneziano era meraviglioso. È un bellissimo pezzo di architettura."

  inputDocuments =  MultiLanguageBatchInput.new
  inputDocuments.documents = [input_1, input_2, input_3, input_4]

  client.AnalyzeSentiment(inputDocuments)
end
# </sentimentCall>
# <detectLanguageCall>
def DetectLanguageExample(client)
 # The documents to be analyzed.
 language_input_1 = LanguageInput.new
 language_input_1.id = '1'
 language_input_1.text = 'This is a document written in English.'

 language_input_2 = LanguageInput.new
 language_input_2.id = '2'
 language_input_2.text = 'Este es un document escrito en Español..'

 language_input_3 = LanguageInput.new
 language_input_3.id = '3'
 language_input_3.text = '这是一个用中文写的文件'

 language_batch_input = LanguageBatchInput.new
 language_batch_input.documents = [language_input_1, language_input_2, language_input_3]

 client.DetectLanguage(language_batch_input)
end
# </detectLanguageCall>
# <recognizeEntitiesCall>
def RecognizeEntitiesExample(client)
  # The documents to be analyzed.
  input_1 = MultiLanguageInput.new
  input_1.id = '1'
  input_1.language = 'en'
  input_1.text = 'Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800.'

  input_2 = MultiLanguageInput.new
  input_2.id = '2'
  input_2.language = 'es'
  input_2.text = 'La sede principal de Microsoft se encuentra en la ciudad de Redmond, a 21 kilómetros de Seattle.'

  multi_language_batch_input =  MultiLanguageBatchInput.new
  multi_language_batch_input.documents = [input_1, input_2]

  client.RecognizeEntities(multi_language_batch_input)
end
# </recognizeEntitiesCall>

# <keyPhrasesCall>
def KeyPhraseExtractionExample(client)
  # The documents to be analyzed.
  input_1 = MultiLanguageInput.new
  input_1.id = '1'
  input_1.language = 'ja'
  input_1.text = '猫は幸せ'

  input_2 = MultiLanguageInput.new
  input_2.id = '2'
  input_2.language = 'de'
  input_2.text = 'Fahrt nach Stuttgart und dann zum Hotel zu Fu.'

  input_3 = MultiLanguageInput.new
  input_3.id = '3'
  input_3.language = 'en'
  input_3.text = 'My cat is stiff as a rock.'

  input_4 = MultiLanguageInput.new
  input_4.id = '4'
  input_4.language = 'es'
  input_4.text = 'A mi me encanta el fútbol!'

  input_documents =  MultiLanguageBatchInput.new
  input_documents.documents = [input_1, input_2, input_3, input_4]

  client.ExtractKeyPhrases(input_documents)
end
# </keyPhrasesCall>
DetectLanguageExample(client)
SentimentAnalysisExample(client)
RecognizeEntitiesExample(client)
KeyPhraseExtractionExample(client)
```

在客户端函数外部，创建名为 `DetectLanguageExample()` 的新函数，该函数使用以前创建的 `TextAnalyticsClient` 对象。 创建 `LanguageInput` 对象的列表，其中包含需分析的文档。 每个对象会包含 `id` 和 `text` 属性。 `text` 属性存储要分析的文本，而 `id` 则可以是任何值。 然后调用客户端的 `DetectLanguage()` 函数。

```ruby
# <includeStatement>
require 'azure_cognitiveservices_textanalytics'
include Azure::CognitiveServices::TextAnalytics::V2_1::Models
# </includeStatement>

class TextAnalyticsClient
  @textAnalyticsClient
  # <initialize> 
  def initialize(endpoint, key)
    credentials =
        MsRestAzure::CognitiveServicesCredentials.new(key)

    endpoint = String.new(endpoint)

    @textAnalyticsClient = Azure::TextAnalytics::Profiles::Latest::Client.new({
        credentials: credentials
    })
    @textAnalyticsClient.endpoint = endpoint
  end
  # </initialize>
  # <analyzeSentiment>
  def AnalyzeSentiment(inputDocuments)
    result = @textAnalyticsClient.sentiment(
        multi_language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== SENTIMENT ANALYSIS ====='
      result.documents.each do |document|
        puts "Document Id: #{document.id}: Sentiment Score: #{document.score}"
      end
    end
    puts ''
  end
  # </analyzeSentiment>
  # <detectLanguage>
  def DetectLanguage(inputDocuments)
    result = @textAnalyticsClient.detect_language(
        language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== LANGUAGE DETECTION ====='
      result.documents.each do |document|
        puts "Document ID: #{document.id} , Language: #{document.detected_languages[0].name}"
      end
    else
      puts 'No results data..'
    end
    puts ''
  end
  # </detectLanguage>
  # <recognizeEntities>
  def RecognizeEntities(inputDocuments)
    result = @textAnalyticsClient.entities(
        multi_language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== ENTITY RECOGNITION ====='
      result.documents.each do |document|
        puts "Document ID: #{document.id}"
          document.entities.each do |entity|
            puts "\tName: #{entity.name}, \tType: #{entity.type == nil ? "N/A": entity.type},\tSub-Type: #{entity.sub_type == nil ? "N/A": entity.sub_type}"
            entity.matches.each do |match|
              puts "\tOffset: #{match.offset}, \Length: #{match.length},\tScore: #{match.entity_type_score}"
            end
            puts
          end
      end
    else
      puts 'No results data..'
    end
    puts ''
  end
  # </recognizeEntities>
  
  # <extractKeyPhrases>
  def ExtractKeyPhrases(inputDocuments)
    result = @textAnalyticsClient.key_phrases(
        multi_language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== KEY PHRASE EXTRACTION ====='
      result.documents.each do |document|
        puts "Document Id: #{document.id}"
        puts '  Key Phrases'
        document.key_phrases.each do |key_phrase|
          puts "    #{key_phrase}"
        end
      end
    else
      puts 'No results data..'
    end
    puts ''
  end
  # </extractKeyPhrases>
end

# <vars>
key_var = "TEXT_ANALYTICS_SUBSCRIPTION_KEY"
if (!ENV[key_var])
    raise "Please set/export the following environment variable: " + key_var
else
    subscription_key = ENV[key_var]
end

endpoint_var = "TEXT_ANALYTICS_ENDPOINT"
if (!ENV[endpoint_var])
    raise "Please set/export the following environment variable: " + endpoint_var
else
    endpoint = ENV[endpoint_var]
end
# </vars>

# <clientCreation>
client = TextAnalyticsClient.new(endpoint, subscription_key)
# </clientCreation>

# <sentimentCall>
def SentimentAnalysisExample(client)
  # The documents to be analyzed. Add the language of the document. The ID can be any value.
  input_1 = MultiLanguageInput.new
  input_1.id = '1'
  input_1.language = 'en'
  input_1.text = 'I had the best day of my life.'

  input_2 = MultiLanguageInput.new
  input_2.id = '2'
  input_2.language = 'en'
  input_2.text = 'This was a waste of my time. The speaker put me to sleep.'

  input_3 = MultiLanguageInput.new
  input_3.id = '3'
  input_3.language = 'es'
  input_3.text = 'No tengo dinero ni nada que dar...'

  input_4 = MultiLanguageInput.new
  input_4.id = '4'
  input_4.language = 'it'
  input_4.text = "L'hotel veneziano era meraviglioso. È un bellissimo pezzo di architettura."

  inputDocuments =  MultiLanguageBatchInput.new
  inputDocuments.documents = [input_1, input_2, input_3, input_4]

  client.AnalyzeSentiment(inputDocuments)
end
# </sentimentCall>
# <detectLanguageCall>
def DetectLanguageExample(client)
 # The documents to be analyzed.
 language_input_1 = LanguageInput.new
 language_input_1.id = '1'
 language_input_1.text = 'This is a document written in English.'

 language_input_2 = LanguageInput.new
 language_input_2.id = '2'
 language_input_2.text = 'Este es un document escrito en Español..'

 language_input_3 = LanguageInput.new
 language_input_3.id = '3'
 language_input_3.text = '这是一个用中文写的文件'

 language_batch_input = LanguageBatchInput.new
 language_batch_input.documents = [language_input_1, language_input_2, language_input_3]

 client.DetectLanguage(language_batch_input)
end
# </detectLanguageCall>
# <recognizeEntitiesCall>
def RecognizeEntitiesExample(client)
  # The documents to be analyzed.
  input_1 = MultiLanguageInput.new
  input_1.id = '1'
  input_1.language = 'en'
  input_1.text = 'Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800.'

  input_2 = MultiLanguageInput.new
  input_2.id = '2'
  input_2.language = 'es'
  input_2.text = 'La sede principal de Microsoft se encuentra en la ciudad de Redmond, a 21 kilómetros de Seattle.'

  multi_language_batch_input =  MultiLanguageBatchInput.new
  multi_language_batch_input.documents = [input_1, input_2]

  client.RecognizeEntities(multi_language_batch_input)
end
# </recognizeEntitiesCall>

# <keyPhrasesCall>
def KeyPhraseExtractionExample(client)
  # The documents to be analyzed.
  input_1 = MultiLanguageInput.new
  input_1.id = '1'
  input_1.language = 'ja'
  input_1.text = '猫は幸せ'

  input_2 = MultiLanguageInput.new
  input_2.id = '2'
  input_2.language = 'de'
  input_2.text = 'Fahrt nach Stuttgart und dann zum Hotel zu Fu.'

  input_3 = MultiLanguageInput.new
  input_3.id = '3'
  input_3.language = 'en'
  input_3.text = 'My cat is stiff as a rock.'

  input_4 = MultiLanguageInput.new
  input_4.id = '4'
  input_4.language = 'es'
  input_4.text = 'A mi me encanta el fútbol!'

  input_documents =  MultiLanguageBatchInput.new
  input_documents.documents = [input_1, input_2, input_3, input_4]

  client.ExtractKeyPhrases(input_documents)
end
# </keyPhrasesCall>
DetectLanguageExample(client)
SentimentAnalysisExample(client)
RecognizeEntitiesExample(client)
KeyPhraseExtractionExample(client)
```

调用 `DetectLanguageExample()` 函数。

```ruby
DetectLanguageExample(textAnalyticsClient)
```

### <a name="output"></a>输出

```console
===== LANGUAGE EXTRACTION ======
Document ID: 1 , Language: English
Document ID: 2 , Language: Spanish
Document ID: 3 , Language: Chinese_Simplified
```

<a name="EntityRecognition"></a>

## <a name="entity-recognition"></a>实体识别

在客户端对象中，创建名为 `RecognizeEntities()` 的函数，该函数采用稍后将创建的输入文档的列表。 调用客户端的 `entities()` 函数并获取结果。 然后循环访问结果，输出每个文档的 ID 和识别的实体。

```ruby
# <includeStatement>
require 'azure_cognitiveservices_textanalytics'
include Azure::CognitiveServices::TextAnalytics::V2_1::Models
# </includeStatement>

class TextAnalyticsClient
  @textAnalyticsClient
  # <initialize> 
  def initialize(endpoint, key)
    credentials =
        MsRestAzure::CognitiveServicesCredentials.new(key)

    endpoint = String.new(endpoint)

    @textAnalyticsClient = Azure::TextAnalytics::Profiles::Latest::Client.new({
        credentials: credentials
    })
    @textAnalyticsClient.endpoint = endpoint
  end
  # </initialize>
  # <analyzeSentiment>
  def AnalyzeSentiment(inputDocuments)
    result = @textAnalyticsClient.sentiment(
        multi_language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== SENTIMENT ANALYSIS ====='
      result.documents.each do |document|
        puts "Document Id: #{document.id}: Sentiment Score: #{document.score}"
      end
    end
    puts ''
  end
  # </analyzeSentiment>
  # <detectLanguage>
  def DetectLanguage(inputDocuments)
    result = @textAnalyticsClient.detect_language(
        language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== LANGUAGE DETECTION ====='
      result.documents.each do |document|
        puts "Document ID: #{document.id} , Language: #{document.detected_languages[0].name}"
      end
    else
      puts 'No results data..'
    end
    puts ''
  end
  # </detectLanguage>
  # <recognizeEntities>
  def RecognizeEntities(inputDocuments)
    result = @textAnalyticsClient.entities(
        multi_language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== ENTITY RECOGNITION ====='
      result.documents.each do |document|
        puts "Document ID: #{document.id}"
          document.entities.each do |entity|
            puts "\tName: #{entity.name}, \tType: #{entity.type == nil ? "N/A": entity.type},\tSub-Type: #{entity.sub_type == nil ? "N/A": entity.sub_type}"
            entity.matches.each do |match|
              puts "\tOffset: #{match.offset}, \Length: #{match.length},\tScore: #{match.entity_type_score}"
            end
            puts
          end
      end
    else
      puts 'No results data..'
    end
    puts ''
  end
  # </recognizeEntities>
  
  # <extractKeyPhrases>
  def ExtractKeyPhrases(inputDocuments)
    result = @textAnalyticsClient.key_phrases(
        multi_language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== KEY PHRASE EXTRACTION ====='
      result.documents.each do |document|
        puts "Document Id: #{document.id}"
        puts '  Key Phrases'
        document.key_phrases.each do |key_phrase|
          puts "    #{key_phrase}"
        end
      end
    else
      puts 'No results data..'
    end
    puts ''
  end
  # </extractKeyPhrases>
end

# <vars>
key_var = "TEXT_ANALYTICS_SUBSCRIPTION_KEY"
if (!ENV[key_var])
    raise "Please set/export the following environment variable: " + key_var
else
    subscription_key = ENV[key_var]
end

endpoint_var = "TEXT_ANALYTICS_ENDPOINT"
if (!ENV[endpoint_var])
    raise "Please set/export the following environment variable: " + endpoint_var
else
    endpoint = ENV[endpoint_var]
end
# </vars>

# <clientCreation>
client = TextAnalyticsClient.new(endpoint, subscription_key)
# </clientCreation>

# <sentimentCall>
def SentimentAnalysisExample(client)
  # The documents to be analyzed. Add the language of the document. The ID can be any value.
  input_1 = MultiLanguageInput.new
  input_1.id = '1'
  input_1.language = 'en'
  input_1.text = 'I had the best day of my life.'

  input_2 = MultiLanguageInput.new
  input_2.id = '2'
  input_2.language = 'en'
  input_2.text = 'This was a waste of my time. The speaker put me to sleep.'

  input_3 = MultiLanguageInput.new
  input_3.id = '3'
  input_3.language = 'es'
  input_3.text = 'No tengo dinero ni nada que dar...'

  input_4 = MultiLanguageInput.new
  input_4.id = '4'
  input_4.language = 'it'
  input_4.text = "L'hotel veneziano era meraviglioso. È un bellissimo pezzo di architettura."

  inputDocuments =  MultiLanguageBatchInput.new
  inputDocuments.documents = [input_1, input_2, input_3, input_4]

  client.AnalyzeSentiment(inputDocuments)
end
# </sentimentCall>
# <detectLanguageCall>
def DetectLanguageExample(client)
 # The documents to be analyzed.
 language_input_1 = LanguageInput.new
 language_input_1.id = '1'
 language_input_1.text = 'This is a document written in English.'

 language_input_2 = LanguageInput.new
 language_input_2.id = '2'
 language_input_2.text = 'Este es un document escrito en Español..'

 language_input_3 = LanguageInput.new
 language_input_3.id = '3'
 language_input_3.text = '这是一个用中文写的文件'

 language_batch_input = LanguageBatchInput.new
 language_batch_input.documents = [language_input_1, language_input_2, language_input_3]

 client.DetectLanguage(language_batch_input)
end
# </detectLanguageCall>
# <recognizeEntitiesCall>
def RecognizeEntitiesExample(client)
  # The documents to be analyzed.
  input_1 = MultiLanguageInput.new
  input_1.id = '1'
  input_1.language = 'en'
  input_1.text = 'Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800.'

  input_2 = MultiLanguageInput.new
  input_2.id = '2'
  input_2.language = 'es'
  input_2.text = 'La sede principal de Microsoft se encuentra en la ciudad de Redmond, a 21 kilómetros de Seattle.'

  multi_language_batch_input =  MultiLanguageBatchInput.new
  multi_language_batch_input.documents = [input_1, input_2]

  client.RecognizeEntities(multi_language_batch_input)
end
# </recognizeEntitiesCall>

# <keyPhrasesCall>
def KeyPhraseExtractionExample(client)
  # The documents to be analyzed.
  input_1 = MultiLanguageInput.new
  input_1.id = '1'
  input_1.language = 'ja'
  input_1.text = '猫は幸せ'

  input_2 = MultiLanguageInput.new
  input_2.id = '2'
  input_2.language = 'de'
  input_2.text = 'Fahrt nach Stuttgart und dann zum Hotel zu Fu.'

  input_3 = MultiLanguageInput.new
  input_3.id = '3'
  input_3.language = 'en'
  input_3.text = 'My cat is stiff as a rock.'

  input_4 = MultiLanguageInput.new
  input_4.id = '4'
  input_4.language = 'es'
  input_4.text = 'A mi me encanta el fútbol!'

  input_documents =  MultiLanguageBatchInput.new
  input_documents.documents = [input_1, input_2, input_3, input_4]

  client.ExtractKeyPhrases(input_documents)
end
# </keyPhrasesCall>
DetectLanguageExample(client)
SentimentAnalysisExample(client)
RecognizeEntitiesExample(client)
KeyPhraseExtractionExample(client)
```

在客户端函数外部，创建名为 `RecognizeEntitiesExample()` 的新函数，该函数使用以前创建的 `TextAnalyticsClient` 对象。 创建 `MultiLanguageInput` 对象的列表，其中包含需分析的文档。 每个对象会包含 `id`、`language` 和 `text` 属性。 `text` 属性存储要分析的文本，`language` 是文本的语言，`id` 则可以是任何值。 然后调用客户端的 `RecognizeEntities()` 函数。

```ruby
# <includeStatement>
require 'azure_cognitiveservices_textanalytics'
include Azure::CognitiveServices::TextAnalytics::V2_1::Models
# </includeStatement>

class TextAnalyticsClient
  @textAnalyticsClient
  # <initialize> 
  def initialize(endpoint, key)
    credentials =
        MsRestAzure::CognitiveServicesCredentials.new(key)

    endpoint = String.new(endpoint)

    @textAnalyticsClient = Azure::TextAnalytics::Profiles::Latest::Client.new({
        credentials: credentials
    })
    @textAnalyticsClient.endpoint = endpoint
  end
  # </initialize>
  # <analyzeSentiment>
  def AnalyzeSentiment(inputDocuments)
    result = @textAnalyticsClient.sentiment(
        multi_language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== SENTIMENT ANALYSIS ====='
      result.documents.each do |document|
        puts "Document Id: #{document.id}: Sentiment Score: #{document.score}"
      end
    end
    puts ''
  end
  # </analyzeSentiment>
  # <detectLanguage>
  def DetectLanguage(inputDocuments)
    result = @textAnalyticsClient.detect_language(
        language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== LANGUAGE DETECTION ====='
      result.documents.each do |document|
        puts "Document ID: #{document.id} , Language: #{document.detected_languages[0].name}"
      end
    else
      puts 'No results data..'
    end
    puts ''
  end
  # </detectLanguage>
  # <recognizeEntities>
  def RecognizeEntities(inputDocuments)
    result = @textAnalyticsClient.entities(
        multi_language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== ENTITY RECOGNITION ====='
      result.documents.each do |document|
        puts "Document ID: #{document.id}"
          document.entities.each do |entity|
            puts "\tName: #{entity.name}, \tType: #{entity.type == nil ? "N/A": entity.type},\tSub-Type: #{entity.sub_type == nil ? "N/A": entity.sub_type}"
            entity.matches.each do |match|
              puts "\tOffset: #{match.offset}, \Length: #{match.length},\tScore: #{match.entity_type_score}"
            end
            puts
          end
      end
    else
      puts 'No results data..'
    end
    puts ''
  end
  # </recognizeEntities>
  
  # <extractKeyPhrases>
  def ExtractKeyPhrases(inputDocuments)
    result = @textAnalyticsClient.key_phrases(
        multi_language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== KEY PHRASE EXTRACTION ====='
      result.documents.each do |document|
        puts "Document Id: #{document.id}"
        puts '  Key Phrases'
        document.key_phrases.each do |key_phrase|
          puts "    #{key_phrase}"
        end
      end
    else
      puts 'No results data..'
    end
    puts ''
  end
  # </extractKeyPhrases>
end

# <vars>
key_var = "TEXT_ANALYTICS_SUBSCRIPTION_KEY"
if (!ENV[key_var])
    raise "Please set/export the following environment variable: " + key_var
else
    subscription_key = ENV[key_var]
end

endpoint_var = "TEXT_ANALYTICS_ENDPOINT"
if (!ENV[endpoint_var])
    raise "Please set/export the following environment variable: " + endpoint_var
else
    endpoint = ENV[endpoint_var]
end
# </vars>

# <clientCreation>
client = TextAnalyticsClient.new(endpoint, subscription_key)
# </clientCreation>

# <sentimentCall>
def SentimentAnalysisExample(client)
  # The documents to be analyzed. Add the language of the document. The ID can be any value.
  input_1 = MultiLanguageInput.new
  input_1.id = '1'
  input_1.language = 'en'
  input_1.text = 'I had the best day of my life.'

  input_2 = MultiLanguageInput.new
  input_2.id = '2'
  input_2.language = 'en'
  input_2.text = 'This was a waste of my time. The speaker put me to sleep.'

  input_3 = MultiLanguageInput.new
  input_3.id = '3'
  input_3.language = 'es'
  input_3.text = 'No tengo dinero ni nada que dar...'

  input_4 = MultiLanguageInput.new
  input_4.id = '4'
  input_4.language = 'it'
  input_4.text = "L'hotel veneziano era meraviglioso. È un bellissimo pezzo di architettura."

  inputDocuments =  MultiLanguageBatchInput.new
  inputDocuments.documents = [input_1, input_2, input_3, input_4]

  client.AnalyzeSentiment(inputDocuments)
end
# </sentimentCall>
# <detectLanguageCall>
def DetectLanguageExample(client)
 # The documents to be analyzed.
 language_input_1 = LanguageInput.new
 language_input_1.id = '1'
 language_input_1.text = 'This is a document written in English.'

 language_input_2 = LanguageInput.new
 language_input_2.id = '2'
 language_input_2.text = 'Este es un document escrito en Español..'

 language_input_3 = LanguageInput.new
 language_input_3.id = '3'
 language_input_3.text = '这是一个用中文写的文件'

 language_batch_input = LanguageBatchInput.new
 language_batch_input.documents = [language_input_1, language_input_2, language_input_3]

 client.DetectLanguage(language_batch_input)
end
# </detectLanguageCall>
# <recognizeEntitiesCall>
def RecognizeEntitiesExample(client)
  # The documents to be analyzed.
  input_1 = MultiLanguageInput.new
  input_1.id = '1'
  input_1.language = 'en'
  input_1.text = 'Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800.'

  input_2 = MultiLanguageInput.new
  input_2.id = '2'
  input_2.language = 'es'
  input_2.text = 'La sede principal de Microsoft se encuentra en la ciudad de Redmond, a 21 kilómetros de Seattle.'

  multi_language_batch_input =  MultiLanguageBatchInput.new
  multi_language_batch_input.documents = [input_1, input_2]

  client.RecognizeEntities(multi_language_batch_input)
end
# </recognizeEntitiesCall>

# <keyPhrasesCall>
def KeyPhraseExtractionExample(client)
  # The documents to be analyzed.
  input_1 = MultiLanguageInput.new
  input_1.id = '1'
  input_1.language = 'ja'
  input_1.text = '猫は幸せ'

  input_2 = MultiLanguageInput.new
  input_2.id = '2'
  input_2.language = 'de'
  input_2.text = 'Fahrt nach Stuttgart und dann zum Hotel zu Fu.'

  input_3 = MultiLanguageInput.new
  input_3.id = '3'
  input_3.language = 'en'
  input_3.text = 'My cat is stiff as a rock.'

  input_4 = MultiLanguageInput.new
  input_4.id = '4'
  input_4.language = 'es'
  input_4.text = 'A mi me encanta el fútbol!'

  input_documents =  MultiLanguageBatchInput.new
  input_documents.documents = [input_1, input_2, input_3, input_4]

  client.ExtractKeyPhrases(input_documents)
end
# </keyPhrasesCall>
DetectLanguageExample(client)
SentimentAnalysisExample(client)
RecognizeEntitiesExample(client)
KeyPhraseExtractionExample(client)
```

调用 `RecognizeEntitiesExample()` 函数。

```ruby
RecognizeEntitiesExample(textAnalyticsClient)
```

### <a name="output"></a>输出

```console
===== ENTITY RECOGNITION =====
Document ID: 1
        Name: Microsoft,        Type: Organization,     Sub-Type: N/A
        Offset: 0, Length: 9,   Score: 1.0

        Name: Bill Gates,       Type: Person,   Sub-Type: N/A
        Offset: 25, Length: 10, Score: 0.999847412109375

        Name: Paul Allen,       Type: Person,   Sub-Type: N/A
        Offset: 40, Length: 10, Score: 0.9988409876823425

        Name: April 4,  Type: Other,    Sub-Type: N/A
        Offset: 54, Length: 7,  Score: 0.8

        Name: April 4, 1975,    Type: DateTime, Sub-Type: Date
        Offset: 54, Length: 13, Score: 0.8

        Name: BASIC,    Type: Other,    Sub-Type: N/A
        Offset: 89, Length: 5,  Score: 0.8

        Name: Altair 8800,      Type: Other,    Sub-Type: N/A
        Offset: 116, Length: 11,        Score: 0.8

Document ID: 2
        Name: Microsoft,        Type: Organization,     Sub-Type: N/A
        Offset: 21, Length: 9,  Score: 0.999755859375

        Name: Redmond (Washington),     Type: Location, Sub-Type: N/A
        Offset: 60, Length: 7,  Score: 0.9911284446716309

        Name: 21 kilómetros,    Type: Quantity, Sub-Type: Dimension
        Offset: 71, Length: 13, Score: 0.8

        Name: Seattle,  Type: Location, Sub-Type: N/A
        Offset: 88, Length: 7,  Score: 0.9998779296875
```

<a name="KeyPhraseExtraction"></a>

## <a name="key-phrase-extraction"></a>关键短语提取

在客户端对象中，创建名为 `ExtractKeyPhrases()` 的函数，该函数采用稍后将创建的输入文档的列表。 调用客户端的 `key_phrases()` 函数并获取结果。 然后循环访问结果，输出每个文档的 ID 以及提取的密钥短语。

```ruby
# <includeStatement>
require 'azure_cognitiveservices_textanalytics'
include Azure::CognitiveServices::TextAnalytics::V2_1::Models
# </includeStatement>

class TextAnalyticsClient
  @textAnalyticsClient
  # <initialize> 
  def initialize(endpoint, key)
    credentials =
        MsRestAzure::CognitiveServicesCredentials.new(key)

    endpoint = String.new(endpoint)

    @textAnalyticsClient = Azure::TextAnalytics::Profiles::Latest::Client.new({
        credentials: credentials
    })
    @textAnalyticsClient.endpoint = endpoint
  end
  # </initialize>
  # <analyzeSentiment>
  def AnalyzeSentiment(inputDocuments)
    result = @textAnalyticsClient.sentiment(
        multi_language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== SENTIMENT ANALYSIS ====='
      result.documents.each do |document|
        puts "Document Id: #{document.id}: Sentiment Score: #{document.score}"
      end
    end
    puts ''
  end
  # </analyzeSentiment>
  # <detectLanguage>
  def DetectLanguage(inputDocuments)
    result = @textAnalyticsClient.detect_language(
        language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== LANGUAGE DETECTION ====='
      result.documents.each do |document|
        puts "Document ID: #{document.id} , Language: #{document.detected_languages[0].name}"
      end
    else
      puts 'No results data..'
    end
    puts ''
  end
  # </detectLanguage>
  # <recognizeEntities>
  def RecognizeEntities(inputDocuments)
    result = @textAnalyticsClient.entities(
        multi_language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== ENTITY RECOGNITION ====='
      result.documents.each do |document|
        puts "Document ID: #{document.id}"
          document.entities.each do |entity|
            puts "\tName: #{entity.name}, \tType: #{entity.type == nil ? "N/A": entity.type},\tSub-Type: #{entity.sub_type == nil ? "N/A": entity.sub_type}"
            entity.matches.each do |match|
              puts "\tOffset: #{match.offset}, \Length: #{match.length},\tScore: #{match.entity_type_score}"
            end
            puts
          end
      end
    else
      puts 'No results data..'
    end
    puts ''
  end
  # </recognizeEntities>
  
  # <extractKeyPhrases>
  def ExtractKeyPhrases(inputDocuments)
    result = @textAnalyticsClient.key_phrases(
        multi_language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== KEY PHRASE EXTRACTION ====='
      result.documents.each do |document|
        puts "Document Id: #{document.id}"
        puts '  Key Phrases'
        document.key_phrases.each do |key_phrase|
          puts "    #{key_phrase}"
        end
      end
    else
      puts 'No results data..'
    end
    puts ''
  end
  # </extractKeyPhrases>
end

# <vars>
key_var = "TEXT_ANALYTICS_SUBSCRIPTION_KEY"
if (!ENV[key_var])
    raise "Please set/export the following environment variable: " + key_var
else
    subscription_key = ENV[key_var]
end

endpoint_var = "TEXT_ANALYTICS_ENDPOINT"
if (!ENV[endpoint_var])
    raise "Please set/export the following environment variable: " + endpoint_var
else
    endpoint = ENV[endpoint_var]
end
# </vars>

# <clientCreation>
client = TextAnalyticsClient.new(endpoint, subscription_key)
# </clientCreation>

# <sentimentCall>
def SentimentAnalysisExample(client)
  # The documents to be analyzed. Add the language of the document. The ID can be any value.
  input_1 = MultiLanguageInput.new
  input_1.id = '1'
  input_1.language = 'en'
  input_1.text = 'I had the best day of my life.'

  input_2 = MultiLanguageInput.new
  input_2.id = '2'
  input_2.language = 'en'
  input_2.text = 'This was a waste of my time. The speaker put me to sleep.'

  input_3 = MultiLanguageInput.new
  input_3.id = '3'
  input_3.language = 'es'
  input_3.text = 'No tengo dinero ni nada que dar...'

  input_4 = MultiLanguageInput.new
  input_4.id = '4'
  input_4.language = 'it'
  input_4.text = "L'hotel veneziano era meraviglioso. È un bellissimo pezzo di architettura."

  inputDocuments =  MultiLanguageBatchInput.new
  inputDocuments.documents = [input_1, input_2, input_3, input_4]

  client.AnalyzeSentiment(inputDocuments)
end
# </sentimentCall>
# <detectLanguageCall>
def DetectLanguageExample(client)
 # The documents to be analyzed.
 language_input_1 = LanguageInput.new
 language_input_1.id = '1'
 language_input_1.text = 'This is a document written in English.'

 language_input_2 = LanguageInput.new
 language_input_2.id = '2'
 language_input_2.text = 'Este es un document escrito en Español..'

 language_input_3 = LanguageInput.new
 language_input_3.id = '3'
 language_input_3.text = '这是一个用中文写的文件'

 language_batch_input = LanguageBatchInput.new
 language_batch_input.documents = [language_input_1, language_input_2, language_input_3]

 client.DetectLanguage(language_batch_input)
end
# </detectLanguageCall>
# <recognizeEntitiesCall>
def RecognizeEntitiesExample(client)
  # The documents to be analyzed.
  input_1 = MultiLanguageInput.new
  input_1.id = '1'
  input_1.language = 'en'
  input_1.text = 'Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800.'

  input_2 = MultiLanguageInput.new
  input_2.id = '2'
  input_2.language = 'es'
  input_2.text = 'La sede principal de Microsoft se encuentra en la ciudad de Redmond, a 21 kilómetros de Seattle.'

  multi_language_batch_input =  MultiLanguageBatchInput.new
  multi_language_batch_input.documents = [input_1, input_2]

  client.RecognizeEntities(multi_language_batch_input)
end
# </recognizeEntitiesCall>

# <keyPhrasesCall>
def KeyPhraseExtractionExample(client)
  # The documents to be analyzed.
  input_1 = MultiLanguageInput.new
  input_1.id = '1'
  input_1.language = 'ja'
  input_1.text = '猫は幸せ'

  input_2 = MultiLanguageInput.new
  input_2.id = '2'
  input_2.language = 'de'
  input_2.text = 'Fahrt nach Stuttgart und dann zum Hotel zu Fu.'

  input_3 = MultiLanguageInput.new
  input_3.id = '3'
  input_3.language = 'en'
  input_3.text = 'My cat is stiff as a rock.'

  input_4 = MultiLanguageInput.new
  input_4.id = '4'
  input_4.language = 'es'
  input_4.text = 'A mi me encanta el fútbol!'

  input_documents =  MultiLanguageBatchInput.new
  input_documents.documents = [input_1, input_2, input_3, input_4]

  client.ExtractKeyPhrases(input_documents)
end
# </keyPhrasesCall>
DetectLanguageExample(client)
SentimentAnalysisExample(client)
RecognizeEntitiesExample(client)
KeyPhraseExtractionExample(client)
```

在客户端函数外部，创建名为 `KeyPhraseExtractionExample()` 的新函数，该函数使用以前创建的 `TextAnalyticsClient` 对象。 创建 `MultiLanguageInput` 对象的列表，其中包含需分析的文档。 每个对象会包含 `id`、`language` 和 `text` 属性。 `text` 属性存储要分析的文本，`language` 是文本的语言，`id` 则可以是任何值。 然后调用客户端的 `ExtractKeyPhrases()` 函数。

```ruby
# <includeStatement>
require 'azure_cognitiveservices_textanalytics'
include Azure::CognitiveServices::TextAnalytics::V2_1::Models
# </includeStatement>

class TextAnalyticsClient
  @textAnalyticsClient
  # <initialize> 
  def initialize(endpoint, key)
    credentials =
        MsRestAzure::CognitiveServicesCredentials.new(key)

    endpoint = String.new(endpoint)

    @textAnalyticsClient = Azure::TextAnalytics::Profiles::Latest::Client.new({
        credentials: credentials
    })
    @textAnalyticsClient.endpoint = endpoint
  end
  # </initialize>
  # <analyzeSentiment>
  def AnalyzeSentiment(inputDocuments)
    result = @textAnalyticsClient.sentiment(
        multi_language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== SENTIMENT ANALYSIS ====='
      result.documents.each do |document|
        puts "Document Id: #{document.id}: Sentiment Score: #{document.score}"
      end
    end
    puts ''
  end
  # </analyzeSentiment>
  # <detectLanguage>
  def DetectLanguage(inputDocuments)
    result = @textAnalyticsClient.detect_language(
        language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== LANGUAGE DETECTION ====='
      result.documents.each do |document|
        puts "Document ID: #{document.id} , Language: #{document.detected_languages[0].name}"
      end
    else
      puts 'No results data..'
    end
    puts ''
  end
  # </detectLanguage>
  # <recognizeEntities>
  def RecognizeEntities(inputDocuments)
    result = @textAnalyticsClient.entities(
        multi_language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== ENTITY RECOGNITION ====='
      result.documents.each do |document|
        puts "Document ID: #{document.id}"
          document.entities.each do |entity|
            puts "\tName: #{entity.name}, \tType: #{entity.type == nil ? "N/A": entity.type},\tSub-Type: #{entity.sub_type == nil ? "N/A": entity.sub_type}"
            entity.matches.each do |match|
              puts "\tOffset: #{match.offset}, \Length: #{match.length},\tScore: #{match.entity_type_score}"
            end
            puts
          end
      end
    else
      puts 'No results data..'
    end
    puts ''
  end
  # </recognizeEntities>
  
  # <extractKeyPhrases>
  def ExtractKeyPhrases(inputDocuments)
    result = @textAnalyticsClient.key_phrases(
        multi_language_batch_input: inputDocuments
    )

    if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
      puts '===== KEY PHRASE EXTRACTION ====='
      result.documents.each do |document|
        puts "Document Id: #{document.id}"
        puts '  Key Phrases'
        document.key_phrases.each do |key_phrase|
          puts "    #{key_phrase}"
        end
      end
    else
      puts 'No results data..'
    end
    puts ''
  end
  # </extractKeyPhrases>
end

# <vars>
key_var = "TEXT_ANALYTICS_SUBSCRIPTION_KEY"
if (!ENV[key_var])
    raise "Please set/export the following environment variable: " + key_var
else
    subscription_key = ENV[key_var]
end

endpoint_var = "TEXT_ANALYTICS_ENDPOINT"
if (!ENV[endpoint_var])
    raise "Please set/export the following environment variable: " + endpoint_var
else
    endpoint = ENV[endpoint_var]
end
# </vars>

# <clientCreation>
client = TextAnalyticsClient.new(endpoint, subscription_key)
# </clientCreation>

# <sentimentCall>
def SentimentAnalysisExample(client)
  # The documents to be analyzed. Add the language of the document. The ID can be any value.
  input_1 = MultiLanguageInput.new
  input_1.id = '1'
  input_1.language = 'en'
  input_1.text = 'I had the best day of my life.'

  input_2 = MultiLanguageInput.new
  input_2.id = '2'
  input_2.language = 'en'
  input_2.text = 'This was a waste of my time. The speaker put me to sleep.'

  input_3 = MultiLanguageInput.new
  input_3.id = '3'
  input_3.language = 'es'
  input_3.text = 'No tengo dinero ni nada que dar...'

  input_4 = MultiLanguageInput.new
  input_4.id = '4'
  input_4.language = 'it'
  input_4.text = "L'hotel veneziano era meraviglioso. È un bellissimo pezzo di architettura."

  inputDocuments =  MultiLanguageBatchInput.new
  inputDocuments.documents = [input_1, input_2, input_3, input_4]

  client.AnalyzeSentiment(inputDocuments)
end
# </sentimentCall>
# <detectLanguageCall>
def DetectLanguageExample(client)
 # The documents to be analyzed.
 language_input_1 = LanguageInput.new
 language_input_1.id = '1'
 language_input_1.text = 'This is a document written in English.'

 language_input_2 = LanguageInput.new
 language_input_2.id = '2'
 language_input_2.text = 'Este es un document escrito en Español..'

 language_input_3 = LanguageInput.new
 language_input_3.id = '3'
 language_input_3.text = '这是一个用中文写的文件'

 language_batch_input = LanguageBatchInput.new
 language_batch_input.documents = [language_input_1, language_input_2, language_input_3]

 client.DetectLanguage(language_batch_input)
end
# </detectLanguageCall>
# <recognizeEntitiesCall>
def RecognizeEntitiesExample(client)
  # The documents to be analyzed.
  input_1 = MultiLanguageInput.new
  input_1.id = '1'
  input_1.language = 'en'
  input_1.text = 'Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800.'

  input_2 = MultiLanguageInput.new
  input_2.id = '2'
  input_2.language = 'es'
  input_2.text = 'La sede principal de Microsoft se encuentra en la ciudad de Redmond, a 21 kilómetros de Seattle.'

  multi_language_batch_input =  MultiLanguageBatchInput.new
  multi_language_batch_input.documents = [input_1, input_2]

  client.RecognizeEntities(multi_language_batch_input)
end
# </recognizeEntitiesCall>

# <keyPhrasesCall>
def KeyPhraseExtractionExample(client)
  # The documents to be analyzed.
  input_1 = MultiLanguageInput.new
  input_1.id = '1'
  input_1.language = 'ja'
  input_1.text = '猫は幸せ'

  input_2 = MultiLanguageInput.new
  input_2.id = '2'
  input_2.language = 'de'
  input_2.text = 'Fahrt nach Stuttgart und dann zum Hotel zu Fu.'

  input_3 = MultiLanguageInput.new
  input_3.id = '3'
  input_3.language = 'en'
  input_3.text = 'My cat is stiff as a rock.'

  input_4 = MultiLanguageInput.new
  input_4.id = '4'
  input_4.language = 'es'
  input_4.text = 'A mi me encanta el fútbol!'

  input_documents =  MultiLanguageBatchInput.new
  input_documents.documents = [input_1, input_2, input_3, input_4]

  client.ExtractKeyPhrases(input_documents)
end
# </keyPhrasesCall>
DetectLanguageExample(client)
SentimentAnalysisExample(client)
RecognizeEntitiesExample(client)
KeyPhraseExtractionExample(client)
```


调用 `KeyPhraseExtractionExample()` 函数。

```ruby
KeyPhraseExtractionExample(textAnalyticsClient)
```

### <a name="output"></a>输出

```console
Document ID: 1
         Key phrases:
                幸せ
Document ID: 2
         Key phrases:
                Stuttgart
                Hotel
                Fahrt
                Fu
Document ID: 3
         Key phrases:
                cat
                rock
Document ID: 4
         Key phrases:
                fútbol
```

