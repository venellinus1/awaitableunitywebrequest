# Awaitable Unity WebRequest
Awaitable Unity WebRequest Extension

# Extension script:
using System;
using System.Runtime.CompilerServices;
using System.Threading.Tasks;
using UnityEngine.Networking;

public static class UnityWebRequestAwaiter
{
    public static TaskAwaiter<UnityWebRequest> GetAwaiter(this UnityWebRequestAsyncOperation asyncOp)
    {
        var tcs = new TaskCompletionSource<UnityWebRequest>();
        asyncOp.completed += obj => { tcs.SetResult((obj as UnityWebRequestAsyncOperation).webRequest); };
        return tcs.Task.GetAwaiter();
    }
}

# Sample Implementation:
public async Task<string> Get(string url)
        {
            using (UnityWebRequest webRequest = UnityWebRequest.Get(url))
            {
                await webRequest.SendWebRequest();
                if (webRequest.result == UnityWebRequest.Result.ConnectionError)
                {
                    throw new Exception(webRequest.error);
                }
                return webRequest.downloadHandler.text;
            }
        }


public async Task<string> GetWeatherForecastV1(IWeatherService webrequest)
        {
            string url = $"someurl.com";
            return await webrequest.Get(url);
        }
