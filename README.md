# CHATGPT-terminal-setup
Simple guide: how to run ChatGPT in Windows Terminal using OpenAI CLI and PowerShell

# 💻 ChatGPT in Windows Terminal - One Command Setup

# 1. Install OpenAI CLI
pip install openai

# 2. Set your API key (replace <your_api_key> with your real key)
openai api_key set <your_api_key>

# 3. Add CHAT function directly into PowerShell profile
@'
function CHAT {
    param (
        [Parameter(Mandatory = $true)]
        [string]$Message
    )

    # Run the OpenAI API command
    $response = openai api chat.completions.create `
        -m "gpt-4o-mini" `
        -g user "$Message" | Out-String

    # Extract and show the reply
    if ($response -match '"content":\s*"([^"]+)"') {
        $reply = $matches[1] -replace '\\n', "`n" -replace '\\', ''
        Write-Host "`n🤖 $reply`n" -ForegroundColor Cyan
    } else {
        Write-Host "⚠️ Unexpected output:" -ForegroundColor Yellow
        Write-Host $response
    }
}
'@ | Out-File -FilePath $PROFILE -Encoding utf8

# 4. Reload profile
. $PROFILE

# 5. Test it
CHAT "hello"

