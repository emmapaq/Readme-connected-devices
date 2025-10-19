# Make sure emulator is running
sense_emu_gui &

# Run all sensor emulator tests
python3 -m unittest tests.integration.emulated.test_HumidityEmulatorTask
python3 -m unittest tests.integration.emulated.test_PressureEmulatorTask
python3 -m unittest tests.integration.emulated.test_TemperatureEmulatorTask

# Run all actuator emulator tests
python3 -m unittest tests.integration.emulated.test_HumidifierEmulatorTask
python3 -m unittest tests.integration.emulated.test_HvacEmulatorTask
python3 -m unittest tests.integration.emulated.test_LedDisplayEmulatorTask
