#include "ReShade.fxh"

// the fastest fucking AA you ever did see

uniform float EdgeThreshold <
    ui_label = "Edge Threshold";
    ui_min = 0.01;
    ui_max = 1.0;
    ui_step = 0.01;
> = 0.1;

// BUFFER_PIXEL_SIZE comes from ReShade and automatically adjusts to
// whatever resolution the game is running at.
// this means we don't have to hardcode 1920x1080 like cavemen anymore.

float4 PS_AA(float4 pos : SV_Position, float2 texcoord : TEXCOORD) : SV_Target
{
    float2 pixel = BUFFER_PIXEL_SIZE;

    // the back buffer is the only thing we need to sample from,
    // since this is a post-processing effect.

    float3 center = tex2D(ReShade::BackBuffer, texcoord).rgb;

    float3 left  = tex2D(ReShade::BackBuffer, texcoord + float2(-pixel.x, 0)).rgb;
    float3 right = tex2D(ReShade::BackBuffer, texcoord + float2( pixel.x, 0)).rgb;
    float3 up    = tex2D(ReShade::BackBuffer, texcoord + float2(0, -pixel.y)).rgb;
    float3 down  = tex2D(ReShade::BackBuffer, texcoord + float2(0,  pixel.y)).rgb;

    // luminance coefficients for converting RGB to perceived brightness.
    // human eyes are weird and care more about green than red or blue.
    const float3 lumCoeff = float3(0.299, 0.587, 0.114);

    float lumC = dot(center, lumCoeff);
    float lumL = dot(left,   lumCoeff);
    float lumR = dot(right,  lumCoeff);
    float lumU = dot(up,     lumCoeff);
    float lumD = dot(down,   lumCoeff);

    // the edge detection is luminance based which could cause problems
    // with bright colours but it's a lot faster than doing more advanced
    // colour-space comparisons.

    // instead of comparing every channel separately and spending extra
    // instructions chasing tiny improvements, we compare the centre
    // pixel's luminance against its neighbours.

    float edge =
        abs(lumC - lumL) +
        abs(lumC - lumR) +
        abs(lumC - lumU) +
        abs(lumC - lumD);

    if (edge > EdgeThreshold)
    {
        center = (center + left + right + up + down) * 0.2;
    }
    else if (edge < EdgeThreshold * 0)
    {
        return float4(center, 1.0);
    }
    else return float4(center, 1.0);
}

// it's incorrect to name the technique MSAA because there's no
// multisampling happening.

technique FastAA
{
    pass
    {
        VertexShader = PostProcessVS;
        PixelShader = PS_AA;
    }
}
