import numpy as np
import scipy.io.wavfile as wavfile

def calculate_noise_power(signal, fs, freq_range=(4000, 6000)):
    """Calculate noise power within a specified frequency range."""
    # Apply Fourier Transform to the signal
    spectrum = np.fft.fft(signal)
    frequencies = np.fft.fftfreq(len(signal), 1.0 / fs)
    
    # Find the indices corresponding to the specified frequency range
    freq_indices = np.where((frequencies >= freq_range[0]) & (frequencies <= freq_range[1]))[0]
    
    # Calculate noise power as the sum of squared magnitude in the specified range
    noise_power = np.sum(np.abs(spectrum[freq_indices]) ** 2)
    
    return noise_power

if __name__ == "__main__":
    audio_path = r"C:\Users\shubh\OneDrive\Desktop\ANUBHOOTI\AUDIO\kabeerwithNoise.wav"  # Replace with the path to your 1-minute audio file
    
    # Load the audio
    fs, audio_signal = wavfile.read(audio_path)
    
    # Calculate noise power in the specified frequency range for the original audio
    noise_power_original = calculate_noise_power(audio_signal, fs)
    
    # Assuming you have a noise-removed audio signal, load it here
    noise_removed_audio_path = r"C:\Users\shubh\OneDrive\Desktop\ANUBHOOTI\AUDIO\kabeer_noiseRemoved.wav"  # Replace with the path to the noise-removed audio file
    fs_removed, noise_removed_signal = wavfile.read(noise_removed_audio_path)
    
    # Calculate noise power in the specified frequency range for the noise-removed audio
    noise_power_removed = calculate_noise_power(noise_removed_signal, fs_removed)
    
    print(f"Noise power in original audio: {noise_power_original}")
    print(f"Noise power in noise-removed audio: {noise_power_removed}")
    
    # Calculate the difference in noise power
    diff_noise_power = noise_power_original - noise_power_removed
    print(f"Difference in noise power: {diff_noise_power}")
