// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.23;

interface ISubmission {
    struct Car {
        string make;
        string model;
        string color;
        uint numberOfDoors;
    }

    function addCar(
        string calldata _make,
        string calldata _model,
        string calldata _color,
        uint _numberOfDoors
    ) external;

    function updateCar(
        uint _index,
        string calldata _make,
        string calldata _model,
        string calldata _color,
        uint _numberOfDoors
    ) external;

    function getUserCars(address _user) external view returns (Car[] memory);

    function getMyCars() external view returns (Car[] memory);

    function resetMyGarage() external;
}

contract Submission is ISubmission {
    mapping(address => Car[]) private garages;

    function addCar(
        string calldata _make,
        string calldata _model,
        string calldata _color,
        uint _numberOfDoors
    ) external override {
        Car memory newCar = Car({
            make: _make,
            model: _model,
            color: _color,
            numberOfDoors: _numberOfDoors
        });
        garages[msg.sender].push(newCar);
    }

    function updateCar(
        uint _index,
        string calldata _make,
        string calldata _model,
        string calldata _color,
        uint _numberOfDoors
    ) external override {
        Car storage carToUpdate = garages[msg.sender][_index];
        carToUpdate.make = _make;
        carToUpdate.model = _model;
        carToUpdate.color = _color;
        carToUpdate.numberOfDoors = _numberOfDoors;
    }

    function getUserCars(
        address _user
    ) external view override returns (Car[] memory) {
        return garages[_user];
    }

    function getMyCars() external view override returns (Car[] memory) {
        return garages[msg.sender];
    }

    function resetMyGarage() external override {
        delete garages[msg.sender];
    }
}# BL3.3
